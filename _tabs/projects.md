---
layout: page
icon: fas fa-code-branch
order: 0
permalink: /projects/
title: Projects
---

C++ 기반 **GPU Simulation / Real-time Graphics**를 중심으로 구현한 개인 프로젝트를 정리하고 있습니다.

Vulkan과 DirectX 12를 사용해 GPU 병렬 처리, Physics Simulation, Rendering Pipeline 및 성능 최적화를 직접 구현하고 분석했습니다.

---

## 💧 DX12 기반 100만 파티클 PBF Fluid Simulation

> **Role:** Solo Developer | **Tech:** C++, DirectX 12, HLSL, Compute Shader | **Period:** 2026.02

[![PBF Fluid Simulation](https://img.youtube.com/vi/OuQbcxNxZGo/maxresdefault.jpg)](https://www.youtube.com/watch?v=OuQbcxNxZGo)

### 💡 Project Overview

DirectX 12 Compute Shader 기반으로 **Position Based Fluids(PBF)** 유체 시뮬레이션과  
**Screen Space Fluid Rendering(SSFR)** 파이프라인을 구현한 프로젝트입니다.

100만 개의 Fluid Particle을 GPU에서 처리하는 것을 목표로 했으며,  
Spatial Hash 기반 Neighbor Search, GPU Sorting, Particle Permutation, Resource Synchronization 및 GPU-Driven Rendering을 구현했습니다.

PIX를 이용해 GPU 병목 구간을 분석하고, 정렬 및 Memory Access 구조를 개선했습니다.

### 🔧 Key Features

- **GPU PBF Solver**
  - Integration
  - Spatial Hashing
  - Density / Lambda
  - Delta Position
  - Constraint Solve
  - Vorticity / Velocity Update

- **Spatial Hash 기반 Neighbor Search**
  - Uniform Grid 기반으로 Particle을 공간 분할
  - Grid Cell Key를 기준으로 Particle을 정렬
  - Particle Data 자체를 Grid 순서로 재배열하여 Neighbor-heavy Pass의 Memory Access 개선

- **GPU Sorting 최적화**
  - 초기 Bitonic Sort 구현
  - Integer Cell Key 특성을 이용해 Counting Sort 기반 Pipeline으로 변경
  - `2^20 Particle` 기준 **5.19 ms → 0.68 ms**
  - Sorting Time 약 **86.9% 감소**

- **GPU Resource / Synchronization**
  - Compute Pass별 UAV / SRV 접근 패턴 분리
  - Resource State Transition 및 Barrier 관리
  - PIX Hardware Counter 기반 GPU Pipeline Profiling

- **Screen Space Fluid Rendering**
  - Linear Depth
  - Separable Bilateral Blur
  - Thickness
  - Normal Reconstruction
  - Reflection / Refraction

- **GPU-Driven Diffuse Particle**
  - ExecuteIndirect 기반 Rendering
  - Active Particle Count의 CPU Readback 제거

### 📊 Performance

- Fluid Particles: **1,000,000**
- PBF Compute: **약 25.2 ms**
- PBF + SSFR GPU Frame: **약 28.4 ms**
- Neighbor Search / Solver Iteration: **약 14 ms**

<div style="text-align: center;">

<a href="https://github.com/steampower33/SPH-PBF-Solver-DX12" class="btn btn-outline-primary btn-lg">📂 GitHub Repository</a>

<a href="https://www.youtube.com/watch?v=kDXEbfrF-uI" class="btn btn-outline-primary btn-lg">▶ Demo</a>

<a href="https://www.youtube.com/watch?v=OuQbcxNxZGo" class="btn btn-outline-primary btn-lg">▶ 1M Performance</a>

<a href="https://app.notion.com/p/DX12-100-PBF-30dfcfd1c8e080eaaa68c4ff985e817f" class="btn btn-outline-primary btn-lg">📄 Technical Write-up</a>

</div>

---

## 🧵 Vulkan 기반 GPU XPBD Cloth Simulation

> **Role:** Solo Developer | **Tech:** C++, Vulkan, GLSL, Compute Shader | **Period:** 2025.10 ~ 2026.01

[![XPBD Cloth Simulation](https://img.youtube.com/vi/nu1VZo1UNBs/maxresdefault.jpg)](https://www.youtube.com/watch?v=nu1VZo1UNBs)

### 💡 Project Overview

Vulkan Compute Shader 기반으로 **XPBD(Extended Position Based Dynamics) Cloth Simulation**을 구현한 프로젝트입니다.

Cloth Constraint Solver를 GPU로 병렬화하면서 발생하는 Write Conflict와 Solver Stability 문제를 다뤘으며,  
Constraint 종류에 따라 **Graph Coloring 기반 Gauss-Seidel-style Solver**와  
**Atomic Add 기반 Jacobi-style Solver**를 사용했습니다.

### 🔧 Key Features

- **XPBD Cloth Solver**
  - Stretch
  - Shear
  - Bend
  - Area Constraint

- **Graph Coloring 기반 Stretch Solver**
  - 동일 Vertex를 공유하지 않는 Constraint를 Color Group으로 구성
  - 같은 Color 내부의 Constraint를 GPU에서 병렬 실행
  - Color Pass 사이 Synchronization 적용
  - In-place Position Update 기반 Gauss-Seidel-style 처리

- **Atomic Jacobi-style Solver**
  - Shear / Bend / Area / Self-Collision Correction을 Atomic Add로 누적
  - 별도의 ApplyDeltas Pass에서 Averaging / Relaxation 후 Position에 적용

- **Self-Collision Broadphase**
  - Spatial Hashing
  - GPU Radix Sort
  - Cell Range 및 Neighbor List 구성

- **Analytic SDF Collision**
  - Sphere
  - Plane
  - Capsule

- **Vulkan Compute / Graphics Pipeline**
  - Compute Shader에서 Cloth Position 갱신
  - 최종 Position을 기반으로 Triangle / Vertex Normal 재구성
  - Rendering Pipeline과 연동

### 📊 Simulation Scale

- Cloth Resolution: **251 × 251**
- Particles: **63,001**
- Generated Constraints: **약 751K**
- Recorded Physics Step: **약 7.4 ms**

> `7.4 ms`는 특정 Scene / Solver Configuration에서 측정한 GPU Physics Workload이며, 전체 Frame Time 또는 실제 FPS를 의미하지 않습니다.

<div style="text-align: center;">

<a href="https://github.com/steampower33/XPBD-Cloth" class="btn btn-outline-primary btn-lg">📂 GitHub Repository</a>

<a href="https://www.youtube.com/watch?v=nu1VZo1UNBs" class="btn btn-outline-primary btn-lg">▶ Demo</a>

<a href="https://app.notion.com/p/Vulkan-GPU-XPBD-Cloth-Simulation-3c9fcfd1c8e080c489c3c7b4b9df6188" class="btn btn-outline-primary btn-lg">📄 Technical Write-up</a>

</div>

---

각 프로젝트의 GitHub Repository에는 실제 구현 코드와 README를 공개하고 있으며,  
Technical Write-up에는 Architecture, GPU 병렬화, 문제 해결 과정 및 성능 최적화 내용을 보다 자세히 정리하고 있습니다.