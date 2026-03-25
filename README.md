<div align="center">

# AZA — Algorithmic Zoning Architect
### *An Experimental Exploration of Voronoi-Based Space Planning for Interior Architecture*

[![Release](https://img.shields.io/github/v/release/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist?style=for-the-badge&color=blue)](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/releases)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D4?style=for-the-badge&logo=windows)](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/releases)
[![License](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Research_/_Experimental-orange?style=for-the-badge)](#-research-context)

---

### 🚀 [DOWNLOAD STANDALONE v0.1.0-ALPHA (.ZIP)](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/releases/download/v0.1.0/AlgorithmicZoningArchitect_v0.1.0.zip)
**Standalone Windows Binary • No Python Required • Zero Dependencies**

---
</div>

## 📌 About This Experiment
**AZA** is a conceptual research instrument that investigates how **weighted Voronoi mass studies** and **capacity-constrained power diagrams** can be applied to early-stage architectural space planning and interior zoning.

In architectural practice, the transition from a written brief into a spatial concept is often intuition-driven. AZA asks: *what if that process could be partially automated through computational geometry, producing concept lines and zone boundaries that respond to area constraints in real time?*

## ✨ Core Capabilities

| Capability | Description |
|---|---|
| **Area-Constrained Solving** | Iterative weight optimization ensures each zone meets its ^2$ target with high precision (<1% error). |
| **Adjacency-Aware Zoning** | An optional matrix encodes P/S/X/N relationships. The solver translates these into attraction and repulsion forces, nudging seeds toward spatially coherent configurations. |
| **Interactive Seed Sculpting** | Drag programmatic anchors across the canvas to reshape the partition in real time with quick-solve feedback (~30ms). |
| **Circulation Gaps** | Adjustable gap width introduces negative-buffer corridors between zones, simulating hallways and partition thickness. |
| **CAD-Ready Export** | Layered **DXF** and **SVG** output for further development in AutoCAD, Rhino, Revit, or Illustrator. |
| **Project Persistence** | Save and load complete floor programs (zones + adjacency) via structured JSON metadata. |

## 🧠 The Computational Engine

### Power Diagram Construction
AZA utilizes the **3D Lifting Trick**: each weighted 2D point is projected onto a paraboloid ( = x^2 + y^2 - w$). The lower convex hull of these lifted points, projected back to 2D, yields the power diagram—a weighted Voronoi partition where boundaries respond to the relative "pressure" of each generator.

### The Iterative Solver Loop
The solver executes a triple-objective optimization per step:
1.  **Weight Adjustment:** Compares actual cell areas against targets, adjusting "pressure" weights with normalized learning rates.
2.  **Adjacency Forces:**
    *   **P — Primary:** (Must share wall/door) Continuous attraction force.
    *   **S — Secondary:** (Should be nearby) Proximity-based attraction.
    *   **X — Conflict:** (Needs separation) Repulsion force for noise/hygiene buffers.
3.  **Lloyd Relaxation:** Gently pulls seeds toward their cell centroids, producing compact, architecturally coherent geometries.

## 📂 Floor Program JSON Format
AZA allows you to load a complete architectural brief from a single JSON file. You can find examples in the examples/ folder.

`jsonc
{
  "zone_categories": {
    "CREATE": { "color": "#9B5DE5", "label": "Workspace" },
    "CHILL":  { "color": "#81B29A", "label": "Wellbeing" }
  },
  "zones": [
    { "id": "GF01", "name": "Lobby", "category": "CREATE", "target_area": 200 }
  ],
  "adjacency": {
    "relationships": [
      ["GF01", "GF02", "P", "Primary circulation connection"]
    ]
  }
}
`

## 🛠 Usage Instructions
1.  **Download & Extract:** Get the latest release and unzip it.
2.  **Launch:** Run AlgorithmicZoningArchitect.exe.
3.  **Define Site:** Input total area or import an **SVG boundary**.
4.  **Program:** Add zones manually or load a JSON brief via File -> Load Floor Program.
5.  **Solve:** Click **"Run Solver"** and watch the area convergence stats in real-time.
6.  **Analyze:** Use Analysis -> Show Adjacency Matrix to verify program satisfaction.

---

## 🔬 Research Context
This work sits at the intersection of **computational design**, **operations research**, and **architectural programming**. It draws on:
*   *Power Diagrams (Aurenhammer, 1987)*
*   *Capacity-Constrained Point Distributions (Balzer et al., 2009)*
*   *Centroidal Voronoi Tessellations (Du et al., 1999)*

## 📄 License
**Proprietary Software Distribution.**
Copyright © 2026 Ibrahim-3d. All Rights Reserved.
This binary distribution is provided for research and evaluation purposes. Unauthorized redistribution or reverse engineering is strictly prohibited.

---
*Developed by Ibrahim-3d • [Report an Issue](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/issues)*
