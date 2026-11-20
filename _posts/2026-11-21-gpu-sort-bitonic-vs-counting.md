---
title: "GPU 정렬: Bitonic Sort에서 Counting Sort로"
description: PBF 이웃 탐색용 Cell Key 정렬을 GPU에서 구현하면서 정리한 Bitonic Sort와 Counting Sort의 구조, 제약, 측정값.
date: 2026-11-21 00:00:00 +0900
categories: [Study, Algorithm]
tags: [sort, gpu, compute-shader, dx12]
math: true
---

[PBF 유체 시뮬레이션](https://github.com/steampower33/SPH-PBF-Solver-DX12)에서는 이웃 탐색을 위해 매 프레임 모든 Particle을 Cell Key(공간 해시 값) 순으로 정렬합니다.
처음에는 Bitonic Sort로 구현했고, Shared Memory 최적화를 거친 뒤 Counting Sort로 바꿨습니다.
그 과정에서 확인한 내용을 정리합니다.

- 측정 환경: RTX 4060 Laptop GPU, PIX GPU Timing
- 정렬 대상: Particle Index 배열 (Key = Cell Hash, 범위 $$ [0, K) $$, $$ K = \text{GridDim}^3 $$)

## 왜 정렬이 필요한가

같은 Cell에 들어 있는 Particle이 메모리에서 연속되도록 만들면, 이웃 탐색 때 "Cell의 시작 위치와 개수"만으로 해당 Cell의 Particle을 차례로 읽을 수 있습니다.
그래서 매 프레임 Key 기준으로 Index를 정렬하고, 그 순서대로 Particle 데이터를 재배열(Permute)합니다.

## 1. Bitonic Sort

Bitonic Sort는 **정렬 네트워크**입니다. 어떤 두 원소를 언제 비교할지가 데이터와 무관하게 미리 정해져 있어서, 각 단계의 비교를 모든 Thread가 동시에 수행할 수 있습니다.

전체 구조는 `blockSize`(병합할 덩어리 크기)와 `stride`(비교할 두 원소의 거리) 두 단계 루프이고, 루프 한 번이 Dispatch 한 번입니다.

```cpp
for (UINT blockSize = 2; blockSize <= N; blockSize <<= 1)
    for (UINT stride = blockSize >> 1; stride > 0; stride >>= 1)
    {
        SortConstants c = { blockSize, stride };
        cmdList->SetComputeRoot32BitConstants(1, 4, &c, 0);
        cmdList->Dispatch((N + 255) / 256, 1, 1);
        // UAV Barrier: 다음 단계는 이번 단계의 결과를 읽는다
    }
```

Shader에서는 Thread `id`가 `id ^ stride` 위치의 원소와 비교하고, `blockSize` 비트로 오름차순 / 내림차순 방향을 정합니다.

```hlsl
uint partner = id ^ g_Stride;
if (partner < id || partner >= numParticles)
    return;

bool ascending = (id & g_BlockSize) == 0;
// Key(Cell Hash)를 비교하고, 같으면 Particle ID로 순서를 정한다
bool swap = (hashA != hashB) ? ((hashA > hashB) == ascending)
                             : ((idA > idB) == ascending);
```

### Dispatch 횟수

$$ N = 2^n $$ 일 때 `blockSize`가 $$ 2^1 \dots 2^n $$, 각각 `stride`가 $$ k $$ 단계이므로 Dispatch 수는

$$
\sum_{k=1}^{n} k = \frac{n(n+1)}{2}
$$

입니다. $$ 2^{18} $$ 개면 171회, $$ 2^{20} $$ 개면 210회입니다. 전체 비교 횟수는 $$ O(N \log^2 N) $$ 입니다.

### 입력 크기는 2의 거듭제곱이어야 한다

이 구현은 $$ N $$ 이 2의 거듭제곱이라고 가정합니다. 측정 중에 실제로 겪은 문제입니다.

- 바깥 루프 조건이 `blockSize <= N` 이라서, 예를 들어 $$ N = 250{,}000 $$ 이면 `blockSize`가 131,072에서 끝나고 마지막 병합 단계(262,144)가 실행되지 않습니다.
- Shader도 `partner >= numParticles` 인 비교를 건너뜁니다.
- 그 결과 **정렬이 덜 된 채로 끝나고, 시간도 실제보다 짧게 측정됩니다.** 에러가 나지 않아서 알아차리기 어렵습니다.

2의 거듭제곱이 아닌 개수를 정렬하려면 다음 2의 거듭제곱까지 최대 Key로 패딩하고 루프를 그 크기까지 돌려야 합니다.

## 2. Shared Memory로 초기 단계 처리하기

Naive 구현은 `stride`가 1, 2, 4처럼 작은 초기 단계에서도 매번 Global Memory 전체를 읽고 쓰고, 그때마다 Dispatch와 Barrier가 붙습니다.
초기 단계는 비교 범위가 작으므로 Thread Group 안에서 끝낼 수 있습니다.

- 1,024 Threads를 한 Group으로 두고, Thread 하나가 원소 2개를 맡아 **2,048개 구간을 `groupshared` 메모리에 올립니다.**
- Key(Cell Hash)도 이때 한 번만 계산해 Shared Memory에 같이 둡니다.
- `blockSize` 2부터 2,048까지의 모든 단계를 Group 안에서 `GroupMemoryBarrierWithGroupSync()`로 동기화하며 처리합니다.
- 다음 병합이 Bitonic 수열을 받을 수 있도록 Group마다 정렬 방향을 번갈아 둡니다(짝수 Group 오름차순, 홀수 Group 내림차순).

```hlsl
bool bGroupAscending = (GID.x % 2 == 0);
for (uint k = 2; k <= 2048; k <<= 1)
    for (uint j = k >> 1; j > 0; j >>= 1)
    {
        uint index   = (tID & ~(j - 1)) * 2 + (tID & (j - 1));
        uint partner = index + j;
        bool ascending = ((index & k) == 0) ^ (!bGroupAscending);
        // s_Hashes / s_Indices 에서 비교 후 교환
        GroupMemoryBarrierWithGroupSync();
    }
```

이후 Global 단계는 `blockSize = 4096`부터 시작합니다. $$ 2^{18} $$ 개 기준 Dispatch는 171회에서 106회(LDS 1회 + Global 105회)로 줄어듭니다.

| $$ N = 2^{18} $$ (262,144) | Dispatch | Bitonic Sort GPU Time |
| --- | ---: | ---: |
| Naive | 171 | 1.60 ms |
| Shared Memory 적용 | 106 | 1.01 ms |

같은 날 같은 장면에서 코드만 바꿔 측정한 값입니다. 다만 같은 조건에서도 실행마다 1.01–1.10 ms 정도 차이가 있었습니다.

## 3. Counting Sort

Particle 수를 $$ 2^{20} $$ 으로 늘리자 Shared Memory를 적용한 Bitonic Sort도 5.19 ms가 걸렸습니다.
여기서 정렬 Key의 성질을 다시 봤습니다. Key는 비교만 할 수 있는 임의의 값이 아니라 **$$ [0, K) $$ 범위의 정수**입니다.
이런 경우에는 비교 없이 개수를 세어 위치를 바로 계산하는 Counting Sort를 쓸 수 있습니다.

| 단계 | 하는 일 | Dispatch 크기 |
| --- | --- | --- |
| ClearCellCount | Cell별 개수 0으로 초기화 | $$ K $$ |
| CountParticlesPerCell | 각 Particle의 Key 계산, `InterlockedAdd`로 Cell 개수 증가 | $$ N $$ |
| PrefixSumLocal | Group(1,024 Cell) 단위 Exclusive Scan | $$ K $$ |
| PrefixSumBlock | Group 합계들의 Scan | 1 Group |
| PrefixSumFinalAdd | Group Offset을 더해 Cell 시작 위치 완성 | $$ K $$ |
| BuildSortedIndices | 시작 위치 + Cell 안 순번 위치에 Index 기록 | $$ N $$ |

입자 수와 상관없이 Dispatch는 6회입니다.

핵심은 Count 단계에서 `InterlockedAdd`가 돌려주는 **이전 값**을 그대로 Cell 안의 순번으로 쓰는 것입니다.
그러면 Scatter 단계는 읽기 한 번, 쓰기 한 번으로 끝납니다.

```hlsl
// CountParticlesPerCell
uint hash = GetGridHash(g_PosPred[i], g_SP.CellSize, g_SP.GridDim);
uint localOff;
InterlockedAdd(g_RW_CellCount[hash], 1, localOff);
g_RW_ParticleCellInfo[i] = uint2(hash, localOff);

// BuildSortedIndices
uint2 info = g_ParticleCellInfo[i];
g_RW_SortedIndices[g_CellStart[info.x] + info.y] = i;
```

정렬 결과로 Particle 데이터를 매 프레임 재배열하기 때문에, 다음 프레임에서는 Thread 번호 `i`가 곧 정렬 전 Particle 번호가 됩니다. 따라서 이전 프레임의 정렬 Index를 따로 읽을 필요가 없습니다.

| $$ N = 2^{20} $$ (1,048,576) | 정렬 GPU Time |
| --- | ---: |
| Bitonic Sort (Shared Memory 적용) | 5.19 ms |
| Counting Sort | 0.68 ms |

> 비교 대상인 Bitonic Sort의 Global 단계 Kernel은 비교할 때마다 Position을 읽어 Hash를 다시 계산합니다. Key를 미리 계산해 두는 최적화를 하지 않은 상태였으므로, 이 차이를 알고리즘 차이만으로 볼 수는 없습니다.

## 4. 비교

| | Bitonic Sort | Counting Sort |
| --- | --- | --- |
| 작업량 | $$ O(N \log^2 N) $$ | $$ O(N + K) $$ |
| Dispatch 수 | $$ n(n+1)/2 $$, LDS 적용 시 줄어듦 | 6회 고정 |
| 입력 크기 | 2의 거듭제곱 (아니면 패딩) | 제약 없음 |
| Key 조건 | 비교 가능하면 됨 | 작은 범위의 정수 |
| 같은 Key 안의 순서 | Particle ID로 결정적 | `InterlockedAdd` 실행 순서에 따라 매번 다름 |

## 남은 한계

- **$$ K $$ 가 $$ N $$ 보다 큽니다.** GridDim이 128이면 $$ K = 128^3 $$, 약 210만으로 Particle 수(약 105만)보다 많습니다. Clear와 Prefix Sum 비용은 $$ N $$ 이 아니라 $$ K $$ 에 비례합니다.
- **PrefixSumBlock은 Thread 하나가 직렬로 더합니다.** Group 합계를 Shared Memory 2,048칸에 올려 처리하므로, 지금 구조로는 $$ K \le 2048 \times 1024 = 2^{21} $$ 까지만 다룰 수 있습니다. 128³이 정확히 이 한계입니다.
- **같은 Cell 안의 순서가 비결정적입니다.** 결과가 Bitwise로 재현되어야 하는 경우에는 맞지 않습니다.
- 측정은 한 장면에서 PIX로 몇 번 잰 값이라, 실행마다 수 % 정도의 차이가 있습니다.
