---
layout: page
icon: fas fa-code-branch
order: 0
permalink: /projects/
title: Projects
---

<style>
.project-meta {
  color: var(--text-muted-color);
  font-size: 0.9rem;
  margin-bottom: 1rem;
}

.project-links {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
  margin: 1.2rem 0 2rem;
}

.project-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;

  padding: 0.5rem 0.85rem;

  border: 1px solid var(--btn-border-color, #aaa);
  border-radius: 8px;

  color: var(--text-color) !important;
  text-decoration: none !important;

  font-size: 0.88rem;
  font-weight: 600;

  transition: background 0.15s ease;
}

.project-btn:hover {
  background: var(--button-bg);
}

.project-thumbnail {
  display: block;
  margin: 1rem 0 1.4rem;
}

.project-thumbnail img {
  width: 100%;
  border-radius: 10px;
}

.project-tech {
  margin: 0.8rem 0 1.2rem;
}

.project-tech code {
  margin-right: 0.25rem;
}

.project-divider {
  margin: 3rem 0;
}
</style>


C++ 기반 **GPU Simulation / Real-time Graphics**를 중심으로 구현한 프로젝트를 정리하고 있습니다.

Vulkan과 DirectX 12를 사용해 GPU 병렬 처리, Physics Simulation, Rendering Pipeline과 성능 최적화를 직접 다뤘습니다.


---

# DX12 기반 100만 파티클 PBF Fluid Simulation

<div class="project-meta">
Solo Project · 2026 · C++ / DirectX 12 / HLSL
</div>

<a
  class="project-thumbnail"
  href="https://www.youtube.com/watch?v=OuQbcxNxZGo"
  target="_blank"
>
  <img
    src="https://img.youtube.com/vi/OuQbcxNxZGo/maxresdefault.jpg"
    alt="DX12 PBF Fluid Simulation"
  >
</a>

DirectX 12 Compute Shader 기반으로 **Position Based Fluids(PBF)** 유체 시뮬레이션을 구현한 프로젝트입니다.

100만 개의 Fluid Particle을 GPU에서 처리하는 것을 목표로 했으며,  
Spatial Hash 기반 Neighbor Search, GPU Sorting, Particle Permutation, SSFR 및 GPU-Driven Diffuse Particle Rendering을 구현했습니다.

<div class="project-tech">

`C++` `DirectX 12` `HLSL` `Compute Shader` `PBF` `Spatial Hash` `Counting Sort` `SSFR` `PIX`

</div>

## 주요 구현

- **GPU PBF Solver**
  - Integration
  - Spatial Hashing
  - Density / Lambda
  - Delta Position
  - Constraint Solve
  - Vorticity / Velocity Update

- **Spatial Hash 기반 Neighbor Search**
  - Uniform Grid 기반으로 Particle을 공간 분할
  - Grid Cell Key를 기준으로 Particle Data 재배열

- **GPU Sorting 최적화**
  - 초기 Bitonic Sort 사용
  - Integer Cell Key 특성을 이용해 Counting Sort Pipeline으로 변경
  - `2^20 Particle` 기준 **5.19 ms → 0.68 ms**
  - Sorting Time 약 **86.9% 감소**

- **Resource / Memory Access 최적화**
  - Compute Pass별 UAV / SRV 접근 패턴 분리
  - Resource State Transition 및 Synchronization 관리
  - PIX 기반 GPU Profiling

- **Screen Space Fluid Rendering**
  - Linear Depth
  - Separable Bilateral Blur
  - Thickness
  - Normal Reconstruction
  - Reflection / Refraction

- **GPU-Driven Diffuse Particle**
  - ExecuteIndirect 사용
  - Active Particle Count CPU Readback 제거

## 성능

- Fluid Particle: **1,000,000**
- PBF Compute: **약 25.2 ms**
- PBF + SSFR GPU Frame: **약 28.4 ms**
- 주요 병목: Neighbor Search / Solver Iteration **약 14 ms**

<div class="project-links">

<a
  class="project-btn"
  href="https://github.com/steampower33/SPH-PBF-Solver-DX12"
  target="_blank"
>
  <i class="fab fa-github"></i>
  GitHub
</a>

<a
  class="project-btn"
  href="https://www.youtube.com/watch?v=kDXEbfrF-uI"
  target="_blank"
>
  ▶ Demo
</a>

<a
  class="project-btn"
  href="https://www.youtube.com/watch?v=OuQbcxNxZGo"
  target="_blank"
>
  ▶ 1M Performance
</a>

</div>


<div class="project-divider"></div>


# Vulkan 기반 GPU XPBD Cloth Simulation

<div class="project-meta">
Solo Project · 2025.10 — 2026.01 · C++ / Vulkan / GLSL
</div>

<a
  class="project-thumbnail"
  href="https://www.youtube.com/watch?v=nu1VZo1UNBs"
  target="_blank"
>
  <img
    src="https://img.youtube.com/vi/nu1VZo1UNBs/maxresdefault.jpg"
    alt="Vulkan XPBD Cloth Simulation"
  >
</a>

Vulkan Compute Shader 기반으로 **XPBD Cloth Simulation**을 구현한 프로젝트입니다.

Constraint Solver를 GPU로 병렬화하면서 발생하는 Write Conflict를 해결하기 위해  
Graph Coloring 기반 Gauss-Seidel-style Solver와 Atomic Add 기반 Jacobi-style Solver를 함께 사용했습니다.

<div class="project-tech">

`C++` `Vulkan` `GLSL` `Compute Shader` `XPBD` `Graph Coloring` `Atomic Add` `Spatial Hash`

</div>

## 주요 구현

- **XPBD Cloth Solver**
  - Stretch
  - Shear
  - Bend
  - Area Constraint

- **Graph Coloring 기반 Stretch Solver**
  - 동일 Vertex를 공유하지 않는 Constraint를 Color Group으로 구성
  - 동일 Color 내부 병렬 실행
  - Color 사이 Barrier 적용
  - In-place Position Update 기반 Gauss-Seidel-style Solver

- **Atomic Jacobi-style Solver**
  - Shear / Bend / Area / Self-Collision Correction을 Atomic Add로 누적
  - ApplyDeltas Pass에서 평균 및 Relaxation 후 Position 반영

- **Self-Collision Broadphase**
  - Spatial Hashing
  - GPU Radix Sort
  - Cell Range / Neighbor List 구성

- **Collision**
  - Sphere SDF
  - Plane SDF
  - Capsule SDF

- **Vulkan Compute / Graphics Pipeline 연동**
  - Compute 결과를 기반으로 Cloth Normal 재구성 및 Rendering

## Simulation 규모

- Cloth Resolution: **251 × 251**
- Particles: **63,001**
- Generated Constraints: **약 751K**
- Recorded Physics Step: **약 7.4 ms**

> 7.4 ms는 특정 Scene / Solver Configuration에서 측정한 GPU Physics Workload이며 전체 Frame Time을 의미하지 않습니다.

<div class="project-links">

<a
  class="project-btn"
  href="https://github.com/steampower33/XPBD-Cloth"
  target="_blank"
>
  <i class="fab fa-github"></i>
  GitHub
</a>

<a
  class="project-btn"
  href="https://www.youtube.com/watch?v=nu1VZo1UNBs"
  target="_blank"
>
  ▶ Demo
</a>

</div>


<div class="project-divider"></div>


# DX12 WCSPH Fluid Simulation Prototype

<div class="project-meta">
Pearl Abyss Graphics Internship · 2025.03 — 2025.05
</div>

<!--
WCSPH 스크린샷이 있다면 아래 img 경로에 넣기.

예:
<img src="/assets/img/projects/wcsph.png" alt="WCSPH Fluid Simulation">

assets/img/projects/wcsph.png 파일도 Repository에 추가.
-->

펄어비스 그래픽스실 인턴 과정에서 진행한  
**DirectX 12 Compute Shader 기반 WCSPH Fluid Simulation Prototype**입니다.

GPU Particle Simulation Pipeline과 Spatial Hash 기반 Neighbor Search를 구현하고,  
WCSPH의 Density / Pressure / Force 계산 Pipeline을 구현했습니다.

<div class="project-tech">

`C++` `DirectX 12` `HLSL` `Compute Shader` `WCSPH` `Uniform Grid` `Spatial Hash` `PIX`

</div>

## 주요 구현

- **GPU Particle Pipeline**
  - Particle Spawn
  - Particle Update
  - Rendering

- **Uniform Grid / Spatial Hash**
  - Particle Neighbor Search
  - Hashing / Grouping
  - Prefix Sum 기반 Cell Range 구성 Prototype

- **WCSPH Solver**
  - Density
  - Pressure (Equation of State)
  - Force
  - Velocity / Position Update

- **GPU Synchronization**
  - UAV Barrier 기반 Compute Pass Synchronization

- **GPU Debugging**
  - PIX를 이용한 Buffer / Compute Pipeline 분석

## 프로젝트 규모 및 한계

- 최대 약 **100K Particle Pipeline Test**
- Solver Stability와 Physical Validation을 충분히 체계화하지 못한 Prototype
- 이후 Vulkan XPBD와 DX12 PBF 프로젝트에서 GPU Simulation과 Solver 구조를 추가로 학습

<div class="project-links">

<a
  class="project-btn"
  href="https://github.com/steampower33/SPH-WCSPH-Solver"
  target="_blank"
>
  <i class="fab fa-github"></i>
  GitHub
</a>

<!--
WCSPH 데모 영상이 있다면 아래 주석을 해제하고 링크 수정

<a
  class="project-btn"
  href="WCSPH_YOUTUBE_URL"
  target="_blank"
>
  ▶ Demo
</a>
-->

</div>