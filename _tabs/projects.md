---
layout: page
icon: fas fa-code-branch
order: 0
permalink: /projects/
title: Projects
---

Here are my personal projects focused on **Graphics Engineering** and **Physics Simulation**.
I enjoy exploring various graphics techniques and physics simulations using low-level APIs (DirectX 12, Vulkan).

---

## 🧵 XPBD Cloth Simulation (Vulkan)
> **Role:** Solo Developer | **Tech:** C++, Vulkan, GLSL, Compute Shader | **Period:** 2025.10 ~ 2025.12

[![XPBD Cloth Simulation](https://img.youtube.com/vi/nu1VZo1UNBs/maxresdefault.jpg)](https://www.youtube.com/watch?v=nu1VZo1UNBs)

### 💡 Project Overview
**"PhysixStudio"** is a GPU-based cloth simulation engine built from scratch using **Vulkan**. It implements the **XPBD (Extended Position Based Dynamics)** algorithm to simulate realistic cloth behavior with high stiffness stability.

### 🔧 Key Features
- **GPU-Driven Hybrid Solver:** Implemented a **Jacobi-style accumulation scheme** using `InterlockedAdd` (AtomicAdd) to maximize GPU parallelism, while retaining **Gauss-Seidel** properties for specific constraints.
- **Robust Stability Control:** Solved the "Jittering Cloth" instability (Zero-Energy Modes) by implementing **Dot-product based Shear Constraints** and **Small-steps XPBD** to handle high stiffness without exploding.
- **XPBD Implementation:** Overcame the stiffness limitations of traditional PBD by introducing **compliance** (inverse stiffness), allowing for physically plausible material behavior independent of iteration count.
- **Explicit Synchronization:** Manually managed Vulkan **Compute-Graphics Queue barriers** and memory barriers to ensure correct execution order between simulation steps and rendering.

<div style="text-align: center;">
  <a href="https://github.com/steampower33/PhysixStudio" class="btn btn-outline-primary btn-lg">📂 View GitHub Repository</a>
</div>

---

## ☁️ Real-time Volumetric Cloud (DX11)
> **Role:** Solo Developer | **Tech:** C++, DirectX 11, HLSL | **Period:** 2026.1 ~ Present

[![Volumetric Cloud](https://img.youtube.com/vi/an71mmn2r7w/maxresdefault.jpg)](https://www.youtube.com/watch?v=an71mmn2r7w)

### 💡 Project Overview
A real-time volumetric cloud renderer based on **Raymarching**. This project aims to reproduce the visual quality of AAA games by understanding the physics of light scattering through participating media.

### 🔧 Key Features
- **GLSL to HLSL Porting:** Migrated WebGL-based volumetric shaders to a native **DirectX 11 (C++)** environment, manually mapping resource bindings (Textures, SRVs).
- **Constant Buffer Optimization:** Minimized CPU-GPU bus traffic by splitting Constant Buffers into `Global` (per-frame) and `Cloud` (event-driven with **Dirty Flags**) updates.
- **Explicit Sampler Management:** Configured distinct sampler states (Linear for clouds, Point for **Blue Noise**) to ensure artifact-free dithering and smooth interpolation.
- **Physically Based Rendering:** Implemented **Raymarching** with **Beer’s Law** and **Henyey-Greenstein** phase function to simulate realistic light scattering/absorption.

<div style="text-align: center;">
  <a href="https://github.com/steampower33/RayMarching-DX" class="btn btn-outline-primary btn-lg">📂 View GitHub Repository</a>
</div>

---

## 🌊 SPH Fluid Simulation (DX12)
> **Role:** Solo Developer | **Tech:** C++, DirectX 12, HLSL, Compute Shader | **Period:** 2025.03 ~ 2025.05

[![SPH Fluid Simulation](https://img.youtube.com/vi/p0LWeu2Y7aQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=p0LWeu2Y7aQ)

### 💡 Project Overview
A Lagrangian fluid simulation implementing **WCSPH (Weakly Compressible Smoothed Particle Hydrodynamics)** using **DirectX 12**. The goal was to master explicit resource management and synchronization in low-level graphics APIs.

### 🔧 Key Features
- **DX12 Compute Architecture:** Built a raw DX12 compute pipeline managing **Root Signatures**, **PSOs**, and **UAV Barriers** for particle updates.
- **Data-Oriented Design:** Implemented and benchmarked **Structure of Arrays (SoA)** layout, proving superior cache performance over AoS in compute shaders.
- **GPU Neighbor Search:** implemented **Spatial Hashing** using **Bitonic Sort** and grid-based grouping for particle interactions.
- **Debugging & Profiling:** Validated synchronization barriers and buffer states using **PIX**, handling explicit resource transitions.

<div style="text-align: center;">
  <a href="https://github.com/steampower33/SPH-WCSPH-Solver" class="btn btn-outline-primary btn-lg">📂 View GitHub Repository</a>
</div>

---