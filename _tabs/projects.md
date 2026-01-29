---
layout: page
icon: fas fa-code-branch
order: 2
permalink: /projects/
title: Projects
---

Here are my personal projects focused on **Graphics Engineering** and **Physics Simulation**.
I enjoy exploring various graphics techniques and physics simulations using low-level APIs (DirectX 12, Vulkan).

---

## 🧵 XPBD Cloth Simulation (Vulkan)
> **Role:** Solo Developer | **Tech:** C++, Vulkan, GLSL, Compute Shader | **Year:** 2025.10 ~ 2025.12

[![XPBD Cloth Simulation](https://img.youtube.com/vi/nu1VZo1UNBs/maxresdefault.jpg)](https://www.youtube.com/watch?v=nu1VZo1UNBs)

### 💡 Project Overview
**"PhysixStudio"** is a GPU-based cloth simulation engine built from scratch using **Vulkan**. It implements the **XPBD (Extended Position Based Dynamics)** algorithm to simulate realistic cloth behavior with high stiffness stability.

### 🔧 Key Features
- **Full GPU Simulation:** All physics calculations (Integration, Constraint Solving) are executed on **Compute Shaders**.
- **XPBD Implementation:** Solved the stiffness limitations of traditional PBD by introducing compliance (inverse stiffness).
- **Parallel Primitive Assembly:** Implemented efficient vertex/index buffer management for real-time rendering.
- **Vulkan Synchronization:** Manually managed **Barriers** and **Fences** to synchronize Compute and Graphics queues.

<div style="text-align: center;">
  <a href="https://github.com/steampower33/PhysixStudio" class="btn btn-outline-primary btn-lg">📂 View GitHub Repository</a>
</div>

---

## ☁️ Real-time Volumetric Cloud (DX11)
> **Role:** Solo Developer | **Tech:** C++, DirectX 11, HLSL | **Year:** 2026.1 ~

[![Volumetric Cloud](https://img.youtube.com/vi/an71mmn2r7w/maxresdefault.jpg)](https://www.youtube.com/watch?v=an71mmn2r7w)

### 💡 Project Overview
A real-time volumetric cloud renderer based on **Raymarching**. This project aims to reproduce the visual quality of AAA games by understanding the physics of light scattering through participating media.

### 🔧 Key Features
- **Raymarching & Noise:** Generated procedural 3D clouds using **Perlin-Worley Noise** and FBM (Fractal Brownian Motion).
- **Physically Based Lighting:** Implemented **Beer’s Law** for light absorption to simulate realistic density and shading.
- **Optimization:** Utilized **Empty Space Skipping** and **Blue Noise Dithering** to improve rendering performance.

<div style="text-align: center;">
  <a href="https://github.com/steampower33/RayMarching-DX" class="btn btn-outline-primary btn-lg">📂 View GitHub Repository</a>
</div>

---

## 🌊 SPH Fluid Simulation (DX12)
> **Role:** Solo Developer | **Tech:** C++, DirectX 12, HLSL, Compute Shader | **Year:** 2025.03 ~ 2025.05

[![SPH Fluid Simulation](https://img.youtube.com/vi/p0LWeu2Y7aQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=p0LWeu2Y7aQ)

### 💡 Project Overview
A Lagrangian fluid simulation implementing **WCSPH (Weakly Compressible Smoothed Particle Hydrodynamics)** using **DirectX 12**. The goal was to master explicit resource management and synchronization in low-level graphics APIs.

### 🔧 Key Features
- **DX12 Compute Pipeline:** Built a complete compute pipeline managing **Root Signatures**, **PSOs**, and **Descriptor Heaps**.
- **Particle System:** Simulated thousands of particles interacting with Navier-Stokes equations (Pressure, Viscosity).
- **Neighbor Search:** Implemented spatial hashing or grid-based neighbor search for performance optimization.
- **Resource Management:** Explicitly managed `Upload`, `Default`, and `Readback` heaps for data transfer between CPU and GPU.

<div style="text-align: center;">
  <a href="https://github.com/steampower33/SPH-WCSPH-Solver" class="btn btn-outline-primary btn-lg">📂 View GitHub Repository</a>
</div>

---