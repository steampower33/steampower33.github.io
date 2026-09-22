---
layout: page
icon: fas fa-code-branch
order: 0
permalink: /projects/
title: Projects
---

<style>
/* =========================================================
   PROJECT PAGE
   ========================================================= */

.projects-page {
  --project-text: var(--text-color, #333);
  --project-muted: #6f7480;
  --project-border: rgba(120, 120, 140, 0.16);

  --pastel-blue: #dcecff;
  --pastel-purple: #eadfff;
  --pastel-pink: #ffe1eb;
  --pastel-green: #dff5e8;
  --pastel-yellow: #fff1c9;

  --pastel-blue-strong: #739bd3;
  --pastel-purple-strong: #9279c8;
  --pastel-pink-strong: #d786a2;
  --pastel-green-strong: #6fa989;

  color: var(--project-text);
}


/* ---------- Intro ---------- */

.projects-hero {
  position: relative;
  overflow: hidden;

  padding: 2.2rem 2.1rem;
  margin-bottom: 2.4rem;

  border: 1px solid var(--project-border);
  border-radius: 24px;

  background:
    radial-gradient(
      circle at 10% 20%,
      rgba(220, 236, 255, 0.9),
      transparent 35%
    ),
    radial-gradient(
      circle at 90% 10%,
      rgba(234, 223, 255, 0.8),
      transparent 32%
    ),
    radial-gradient(
      circle at 80% 90%,
      rgba(223, 245, 232, 0.75),
      transparent 35%
    ),
    rgba(255, 255, 255, 0.7);
}

.projects-hero h1 {
  margin: 0 0 0.8rem;
  font-size: 2rem;
  font-weight: 800;
  letter-spacing: -0.035em;
}

.projects-hero p {
  max-width: 720px;
  margin: 0;

  line-height: 1.75;
  color: var(--project-muted);
  font-size: 1rem;
}

.projects-hero strong {
  color: var(--project-text);
}


/* ---------- Section ---------- */

.project-section-title {
  margin: 2.5rem 0 1.2rem;

  font-size: 1.35rem;
  font-weight: 800;
  letter-spacing: -0.025em;
}


/* ---------- Card Grid ---------- */

.project-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 1.25rem;
}

.project-card {
  overflow: hidden;

  border: 1px solid var(--project-border);
  border-radius: 22px;

  background: rgba(255, 255, 255, 0.86);

  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    border-color 0.2s ease;
}

.project-card:hover {
  transform: translateY(-4px);

  border-color: rgba(130, 140, 170, 0.28);

  box-shadow:
    0 16px 36px
    rgba(80, 90, 120, 0.10);
}

.project-card.featured {
  grid-column: 1 / -1;
}


/* ---------- Thumbnail ---------- */

.project-thumbnail {
  position: relative;
  overflow: hidden;

  display: block;

  width: 100%;
  aspect-ratio: 16 / 8.5;

  background: #eef1f6;
}

.project-thumbnail img {
  width: 100%;
  height: 100%;

  object-fit: cover;

  transition: transform 0.28s ease;
}

.project-card:hover .project-thumbnail img {
  transform: scale(1.025);
}

.project-thumbnail::after {
  content: "";

  position: absolute;
  inset: 0;

  background:
    linear-gradient(
      to top,
      rgba(25, 30, 50, 0.18),
      transparent 45%
    );

  pointer-events: none;
}

.video-badge {
  position: absolute;
  right: 14px;
  bottom: 14px;
  z-index: 2;

  padding: 0.38rem 0.72rem;

  border-radius: 999px;

  background: rgba(255, 255, 255, 0.91);

  color: #555d70;
  font-size: 0.78rem;
  font-weight: 700;

  backdrop-filter: blur(8px);
}


/* ---------- Card Body ---------- */

.project-body {
  padding: 1.45rem 1.5rem 1.55rem;
}

.project-topline {
  display: flex;
  align-items: center;
  flex-wrap: wrap;

  gap: 0.6rem;

  margin-bottom: 0.65rem;
}

.project-type {
  display: inline-flex;
  align-items: center;

  padding: 0.28rem 0.65rem;

  border-radius: 999px;

  font-size: 0.72rem;
  font-weight: 800;
}

.type-blue {
  background: var(--pastel-blue);
  color: #557cab;
}

.type-purple {
  background: var(--pastel-purple);
  color: #765daa;
}

.type-green {
  background: var(--pastel-green);
  color: #507f67;
}

.type-pink {
  background: var(--pastel-pink);
  color: #ad617b;
}

.project-period {
  color: var(--project-muted);
  font-size: 0.78rem;
  font-weight: 600;
}

.project-title {
  margin: 0 0 0.55rem;

  font-size: 1.28rem;
  font-weight: 850;
  letter-spacing: -0.025em;
}

.project-description {
  margin: 0 0 1.1rem;

  color: var(--project-muted);
  line-height: 1.7;
  font-size: 0.92rem;
}


/* ---------- Stats ---------- */

.project-stats {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));

  gap: 0.65rem;

  margin: 1.15rem 0;
}

.project-stat {
  padding: 0.8rem 0.72rem;

  border-radius: 14px;

  background: rgba(242, 244, 249, 0.72);

  text-align: center;
}

.project-stat strong {
  display: block;

  margin-bottom: 0.18rem;

  font-size: 0.95rem;
  font-weight: 850;
}

.project-stat span {
  color: var(--project-muted);
  font-size: 0.7rem;
}


/* ---------- Tech Chips ---------- */

.tech-list {
  display: flex;
  flex-wrap: wrap;

  gap: 0.45rem;

  margin: 1rem 0 1.15rem;
}

.tech-chip {
  display: inline-flex;
  align-items: center;

  padding: 0.34rem 0.62rem;

  border-radius: 9px;

  background: rgba(235, 238, 245, 0.72);

  color: #62697a;

  font-size: 0.72rem;
  font-weight: 700;
}


/* ---------- Key Points ---------- */

.project-points {
  margin: 1rem 0 1.25rem;
  padding-left: 1.15rem;

  color: var(--project-muted);
  font-size: 0.88rem;
  line-height: 1.7;
}

.project-points li {
  margin-bottom: 0.4rem;
}

.project-points strong {
  color: var(--project-text);
}


/* ---------- Buttons ---------- */

.project-links {
  display: flex;
  flex-wrap: wrap;

  gap: 0.55rem;
}

.project-link {
  display: inline-flex;
  align-items: center;
  justify-content: center;

  gap: 0.4rem;

  min-height: 38px;
  padding: 0.5rem 0.82rem;

  border: 1px solid var(--project-border);
  border-radius: 11px;

  background: rgba(255, 255, 255, 0.74);

  color: var(--project-text) !important;
  text-decoration: none !important;

  font-size: 0.78rem;
  font-weight: 750;

  transition:
    background 0.18s ease,
    transform 0.18s ease;
}

.project-link:hover {
  background: rgba(235, 239, 247, 0.92);

  transform: translateY(-1px);
}


/* ---------- Footer Note ---------- */

.projects-note {
  margin-top: 2rem;
  padding: 1rem 1.15rem;

  border: 1px solid var(--project-border);
  border-radius: 16px;

  background:
    linear-gradient(
      135deg,
      rgba(220, 236, 255, 0.42),
      rgba(234, 223, 255, 0.42)
    );

  color: var(--project-muted);

  line-height: 1.7;
  font-size: 0.85rem;
}


/* ---------- Dark Mode ---------- */

html[data-mode="dark"] .projects-page {
  --project-text: #e7e9ee;
  --project-muted: #aeb4c2;
  --project-border: rgba(220, 225, 240, 0.10);
}

html[data-mode="dark"] .projects-hero,
html[data-mode="dark"] .project-card {
  background: rgba(35, 38, 48, 0.72);
}

html[data-mode="dark"] .project-stat,
html[data-mode="dark"] .tech-chip {
  background: rgba(255, 255, 255, 0.06);
}

html[data-mode="dark"] .project-link {
  background: rgba(255, 255, 255, 0.045);
}

html[data-mode="dark"] .project-link:hover {
  background: rgba(255, 255, 255, 0.09);
}


/* ---------- Responsive ---------- */

@media (max-width: 760px) {
  .projects-hero {
    padding: 1.6rem 1.35rem;
  }

  .projects-hero h1 {
    font-size: 1.6rem;
  }

  .project-grid {
    grid-template-columns: 1fr;
  }

  .project-card.featured {
    grid-column: auto;
  }

  .project-stats {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 440px) {
  .project-stats {
    grid-template-columns: 1fr;
  }

  .project-body {
    padding: 1.25rem;
  }
}
</style>


<div class="projects-page">

  <!-- =====================================================
       HERO
       ===================================================== -->

  <section class="projects-hero">
    <h1>Graphics & Simulation Projects</h1>

    <p>
      <strong>C++ 기반 GPU Simulation과 Real-time Graphics</strong>를 중심으로
      공부하고 구현한 프로젝트를 정리하고 있습니다.
      Vulkan과 DirectX 12의 저수준 Graphics API를 사용해
      GPU 병렬화, 물리 시뮬레이션, 렌더링 파이프라인과
      성능 최적화를 직접 다뤘습니다.
    </p>
  </section>


  <h2 class="project-section-title">대표 프로젝트</h2>

  <section class="project-grid">


    <!-- ===================================================
         PBF
         =================================================== -->

    <article class="project-card featured">

      <a
        class="project-thumbnail"
        href="https://www.youtube.com/watch?v=OuQbcxNxZGo"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img
          src="https://img.youtube.com/vi/OuQbcxNxZGo/maxresdefault.jpg"
          alt="DX12 PBF Fluid Simulation"
        >
        <span class="video-badge">▶ Performance Demo</span>
      </a>

      <div class="project-body">

        <div class="project-topline">
          <span class="project-type type-blue">GPU Fluid Simulation</span>
          <span class="project-period">2026</span>
        </div>

        <h3 class="project-title">
          DX12 기반 100만 파티클 PBF Fluid Simulation
        </h3>

        <p class="project-description">
          DirectX 12 Compute Shader를 이용해
          Position Based Fluids(PBF) 기반 유체 시뮬레이션과
          Screen Space Fluid Rendering을 구현한 프로젝트입니다.
          Spatial Hash, GPU Sorting, Particle Permutation과
          GPU-Driven Rendering 파이프라인을 구성하고 PIX를 이용해
          병목 구간을 분석하고 최적화했습니다.
        </p>

        <div class="project-stats">
          <div class="project-stat">
            <strong>1,000,000</strong>
            <span>Fluid Particles</span>
          </div>

          <div class="project-stat">
            <strong>28.4 ms</strong>
            <span>PBF + SSFR Baseline</span>
          </div>

          <div class="project-stat">
            <strong>5.19 → 0.68 ms</strong>
            <span>GPU Sorting</span>
          </div>
        </div>

        <div class="tech-list">
          <span class="tech-chip">C++</span>
          <span class="tech-chip">DirectX 12</span>
          <span class="tech-chip">HLSL</span>
          <span class="tech-chip">Compute Shader</span>
          <span class="tech-chip">PBF</span>
          <span class="tech-chip">Spatial Hash</span>
          <span class="tech-chip">Counting Sort</span>
          <span class="tech-chip">SSFR</span>
          <span class="tech-chip">PIX</span>
        </div>

        <ul class="project-points">
          <li>
            <strong>GPU PBF Solver</strong> —
            Integration, Spatial Hashing, Density/Lambda,
            Delta Position, Constraint Solve 파이프라인 구현
          </li>

          <li>
            <strong>GPU Sorting 최적화</strong> —
            2²⁰ Particle 기준 Bitonic Sort
            5.19 ms → Counting Sort 0.68 ms
          </li>

          <li>
            <strong>Resource / Memory 최적화</strong> —
            UAV/SRV 사용 패턴과 Resource State를 Pass 단위로 재구성
          </li>

          <li>
            <strong>GPU-Driven Rendering</strong> —
            ExecuteIndirect를 활용해 Diffuse Particle의
            CPU Readback 제거
          </li>

          <li>
            <strong>SSFR</strong> —
            Linear Depth, Bilateral Blur, Thickness,
            Normal Reconstruction, Refraction/Reflection 구현
          </li>
        </ul>

        <div class="project-links">
          <a
            class="project-link"
            href="https://github.com/steampower33/SPH-PBF-Solver-DX12"
            target="_blank"
            rel="noopener noreferrer"
          >
            <i class="fab fa-github"></i>
            GitHub
          </a>

          <a
            class="project-link"
            href="https://www.youtube.com/watch?v=kDXEbfrF-uI"
            target="_blank"
            rel="noopener noreferrer"
          >
            ▶ Demo
          </a>

          <a
            class="project-link"
            href="https://www.youtube.com/watch?v=OuQbcxNxZGo"
            target="_blank"
            rel="noopener noreferrer"
          >
            ▶ 1M Performance
          </a>
        </div>

      </div>
    </article>


    <!-- ===================================================
         XPBD
         =================================================== -->

    <article class="project-card">

      <a
        class="project-thumbnail"
        href="https://www.youtube.com/watch?v=nu1VZo1UNBs"
        target="_blank"
        rel="noopener noreferrer"
      >
        <img
          src="https://img.youtube.com/vi/nu1VZo1UNBs/maxresdefault.jpg"
          alt="Vulkan XPBD Cloth Simulation"
        >
        <span class="video-badge">▶ Demo</span>
      </a>

      <div class="project-body">

        <div class="project-topline">
          <span class="project-type type-purple">GPU Cloth Simulation</span>
          <span class="project-period">2025.10 — 2026.01</span>
        </div>

        <h3 class="project-title">
          Vulkan 기반 GPU XPBD Cloth Simulation
        </h3>

        <p class="project-description">
          Vulkan Compute Shader 기반으로 XPBD Cloth Solver를 구현하고,
          GPU 병렬화 과정에서 발생하는 Write Conflict와
          Constraint Solver 구조를 다룬 프로젝트입니다.
        </p>

        <div class="project-stats">
          <div class="project-stat">
            <strong>63,001</strong>
            <span>Particles</span>
          </div>

          <div class="project-stat">
            <strong>≈ 751K</strong>
            <span>Constraints</span>
          </div>

          <div class="project-stat">
            <strong>≈ 7.4 ms</strong>
            <span>Recorded Physics Step*</span>
          </div>
        </div>

        <div class="tech-list">
          <span class="tech-chip">C++</span>
          <span class="tech-chip">Vulkan</span>
          <span class="tech-chip">GLSL</span>
          <span class="tech-chip">Compute Shader</span>
          <span class="tech-chip">XPBD</span>
          <span class="tech-chip">Graph Coloring</span>
          <span class="tech-chip">Atomic Add</span>
          <span class="tech-chip">Spatial Hash</span>
        </div>

        <ul class="project-points">
          <li>
            <strong>Graph-colored GS-style Solver</strong> —
            Stretch Constraint의 Write Conflict를 Coloring으로 제거
          </li>

          <li>
            <strong>Atomic Jacobi-style Solver</strong> —
            Shear / Bend / Area / Self-Collision Correction 병렬 누적
          </li>

          <li>
            <strong>Self-Collision Broadphase</strong> —
            Spatial Hashing과 GPU Radix Sort 기반 Neighbor 구성
          </li>

          <li>
            <strong>Analytic SDF Collision</strong> —
            Sphere / Plane / Capsule Collision 구현
          </li>
        </ul>

        <div class="project-links">
          <a
            class="project-link"
            href="https://github.com/steampower33/XPBD-Cloth"
            target="_blank"
            rel="noopener noreferrer"
          >
            <i class="fab fa-github"></i>
            GitHub
          </a>

          <a
            class="project-link"
            href="https://www.youtube.com/watch?v=nu1VZo1UNBs"
            target="_blank"
            rel="noopener noreferrer"
          >
            ▶ Demo
          </a>
        </div>

        <p
          style="
            margin: 0.9rem 0 0;
            font-size: 0.68rem;
            color: var(--project-muted);
          "
        >
          * 특정 Scene / Solver Configuration에서 측정한
          GPU Physics Workload입니다.
        </p>

      </div>
    </article>


    <!-- ===================================================
         WCSPH
         =================================================== -->

    <article class="project-card">

      <div
        class="project-thumbnail"
        style="
          display:flex;
          align-items:center;
          justify-content:center;
          background:
            linear-gradient(
              135deg,
              var(--pastel-green),
              var(--pastel-blue)
            );
        "
      >
        <div
          style="
            font-size:4rem;
            opacity:.72;
          "
        >
          💧
        </div>
      </div>

      <div class="project-body">

        <div class="project-topline">
          <span class="project-type type-green">Graphics Internship</span>
          <span class="project-period">2025.03 — 2025.05</span>
        </div>

        <h3 class="project-title">
          DX12 WCSPH Fluid Simulation Prototype
        </h3>

        <p class="project-description">
          펄어비스 그래픽스실 인턴 과정에서 진행한
          DirectX 12 Compute Shader 기반 WCSPH Fluid Simulation
          Prototype입니다.
          GPU Particle Pipeline과 Spatial Hash 기반 Neighbor Search,
          Density / Pressure / Force 계산을 구현했습니다.
        </p>

        <div class="project-stats">
          <div class="project-stat">
            <strong>≈ 100K</strong>
            <span>Particle Pipeline Test</span>
          </div>

          <div class="project-stat">
            <strong>DX12</strong>
            <span>Compute Pipeline</span>
          </div>

          <div class="project-stat">
            <strong>PIX</strong>
            <span>GPU Debugging</span>
          </div>
        </div>

        <div class="tech-list">
          <span class="tech-chip">C++</span>
          <span class="tech-chip">DirectX 12</span>
          <span class="tech-chip">HLSL</span>
          <span class="tech-chip">WCSPH</span>
          <span class="tech-chip">Uniform Grid</span>
          <span class="tech-chip">Spatial Hash</span>
          <span class="tech-chip">PIX</span>
        </div>

        <ul class="project-points">
          <li>
            <strong>GPU Particle Pipeline</strong> —
            Spawn / Update / Rendering Pipeline 구현
          </li>

          <li>
            <strong>Neighbor Search</strong> —
            Uniform Grid / Spatial Hash 기반 탐색 구조
          </li>

          <li>
            <strong>WCSPH</strong> —
            Density → Pressure(EOS) → Force 계산 Pipeline
          </li>

          <li>
            <strong>프로젝트 한계</strong> —
            Solver Stability와 Physical Validation을 충분히
            체계화하지 못했고, 이후 XPBD/PBF 프로젝트에서 이를 보완
          </li>
        </ul>

        <div class="project-links">
          <a
            class="project-link"
            href="https://github.com/steampower33/SPH-WCSPH-Solver"
            target="_blank"
            rel="noopener noreferrer"
          >
            <i class="fab fa-github"></i>
            GitHub
          </a>
        </div>

      </div>
    </article>

  </section>


  <!-- =====================================================
       FOOTER
       ===================================================== -->

  <div class="projects-note">
    프로젝트의 구현 과정과 기술적인 문제 해결 과정은
    각 GitHub Repository의 README에 보다 자세히 정리하고 있습니다.
    새로운 Graphics / GPU Simulation 프로젝트도 지속적으로 추가할 예정입니다.
  </div>

</div>