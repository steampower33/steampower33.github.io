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