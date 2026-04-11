<div align="center">

# AZA — Algorithmic Zoning Architect
### *An Experimental Exploration of Voronoi-Based Space Planning for Interior Architecture*

[![Release](https://img.shields.io/github/v/release/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist?style=for-the-badge&color=blue)](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/releases)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D4?style=for-the-badge&logo=windows)](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/releases)
[![License](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Research_/_Experimental-orange?style=for-the-badge)](#-research-context)

---

### [DOWNLOAD STANDALONE v0.1.0-ALPHA (.ZIP)](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/releases/download/v0.1.0/AlgorithmicZoningArchitect_v0.1.0.zip)
**Standalone Windows Binary | No Python Required | Zero Dependencies**

---
</div>

## About This Experiment
**AZA** is a conceptual research instrument that investigates how **weighted Voronoi mass studies** and **capacity-constrained power diagrams** can be applied to early-stage architectural space planning and interior zoning.

In architectural practice, the transition from a written brief into a spatial concept is often intuition-driven. AZA asks: *what if that process could be partially automated through computational geometry, producing concept lines and zone boundaries that respond to area constraints in real time?*

## Core Capabilities

| Capability | Description |
|---|---|
| **Area-Constrained Solving** | Iterative weight optimization ensures each zone meets its m² target with high precision (<1% error). Real-time convergence stats. |
| **Adjacency-Aware Zoning** | P/S/X/N relationship matrix translates into attraction and repulsion forces — zones that must share a wall are pulled together; conflicting zones are pushed apart. Satisfaction score after each solve. |
| **Subzones (Nested Zones)** | Two-level zone hierarchy with a deductive area model — children are carved out of the parent's area. A mini power diagram partitions each parent cell among its children. |
| **Per-Zone Corridor Widths** | Each zone can override the global gap width, enabling mixed-width circulation within the same solve. |
| **Column Alignment** | Import structural column positions from SVG. A configurable weight nudges seeds to align with the structural grid. |
| **Entrance Access** | Place entrance points on the boundary. Entrance weight pulls circulation-sensitive zones toward access points. |
| **Aspect Ratio Constraint** | Prevents spaghetti-shaped cells via a corrective force when the oriented bounding rectangle exceeds a configurable length/width threshold. |
| **Cell Locking** | Lock any zone's seed so the solver skips it during optimization while still claiming area and influencing neighbors. Ideal for Cores, MEP risers, stairs, and voids. |
| **Passthrough Spines** | Flag zones as circulation corridors — PCA finds the dominant axis and buffers a clear path through the zone. |
| **Dual Circulation Networks** | Assign zones to "public" or "service" spine networks with conflict detection for shared walls without buffer. |
| **dB-Based Auto-Buffer** | Acoustic noise ratings per zone. Pre-solve computes pairwise dB deltas and sizes corridor buffers automatically. Critical conflicts rendered with red outlines. |
| **Interactive Seed Manipulation** | Drag anchors to reshape the partition in real time (~30 ms quick solve). Parent zones carry children when dragged. |
| **Multi-Select & Group Drag** | Shift+Click or box-drag to select multiple seeds. Dragging moves the whole group. |
| **World-Space Labels** | Zone name and area labels scale naturally with zoom, proportional to cell geometry. HUD panels (stats, legends) remain screen-anchored. |
| **Label Collision Resolver** | Iterative bounding-rect repulsion prevents overlapping labels. Leader lines connect displaced labels to their zone centroids. |
| **CAD-Ready Export** | Layered DXF and SVG output for AutoCAD, Rhino, Revit, or Illustrator. |
| **Vector PDF Export** | Multi-page PDF (parents / subzones / combined) with native vector text and polygons — searchable and crisp at any zoom. |
| **High-DPI Image Export** | Clean white-background PNG/JPEG at 4x resolution, matching SVG colour palette. |
| **Floor Program Import** | Load a complete architectural brief — zones, categories, areas, adjacency, subzone hierarchy, locked state, passthrough spines, noise dB — from a structured JSON file. |
| **Project Persistence** | Save and load full project state including zones, subzone hierarchy, columns, entrances, per-zone settings, and solver config. |
| **Keyboard Shortcuts** | Ctrl+S (save), Ctrl+O (open), Ctrl+Z (undo), Delete (remove zone), Escape (cancel), F (fit view). |
| **Dockable Panel System** | Float, re-dock, and toggle all major panels via the Windows menu. QPainter vector icons — no image resources. |

## The Computational Engine

### Power Diagram Construction
AZA utilizes the **3D Lifting Trick**: each weighted 2D point is projected onto a paraboloid (z = x² + y² - w). The lower convex hull of these lifted points, projected back to 2D, yields the power diagram — a weighted Voronoi partition where boundaries respond to the relative "pressure" of each generator.

### The Iterative Solver Loop
The solver executes a multi-objective optimization per step:
1.  **Weight Adjustment:** Compares actual cell areas against targets, adjusting "pressure" weights with normalized learning rates.
2.  **Adjacency Forces:**
    *   **P — Primary:** (Must share wall/door) Continuous attraction force.
    *   **S — Secondary:** (Should be nearby) Proximity-based attraction.
    *   **X — Conflict:** (Needs separation) Repulsion force for noise/hygiene buffers.
3.  **Column Alignment Forces:** Pulls seeds toward structural grid positions.
4.  **Entrance Access Forces:** Pulls seeds toward boundary access points.
5.  **Aspect Ratio Forces:** Corrective push perpendicular to the long axis of elongated cells.
6.  **Lloyd Relaxation:** Gently pulls seeds toward cell centroids for compact geometries.
7.  **Cell Locking:** Frozen seeds still tessellate and adjust weights — zero positional drift guaranteed.
8.  **Convergence:** Iterates until max area error < 1% or iteration limit reached. Reports adjacency satisfaction and column alignment scores.

## Floor Program JSON Format
AZA loads a complete architectural brief from a single JSON file.

```jsonc
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
```

## Usage Instructions
1.  **Download & Extract:** Get the latest release and unzip it.
2.  **Launch:** Run `AlgorithmicZoningArchitect.exe`.
3.  **Define Site:** Input total area or import an **SVG boundary**.
4.  **Program:** Add zones manually or load a JSON brief via File > Load Floor Program.
5.  **Solve:** Click **"Run Solver"** and watch the area convergence stats in real-time.
6.  **Export:** DXF, SVG, multi-page PDF, or high-DPI PNG/JPEG.

---

## Research Context
This work sits at the intersection of **computational design**, **operations research**, and **architectural programming**. It draws on:
*   *Power Diagrams (Aurenhammer, 1987)*
*   *Capacity-Constrained Point Distributions (Balzer et al., 2009)*
*   *Centroidal Voronoi Tessellations (Du et al., 1999)*

## License
**Proprietary Software Distribution.**
Copyright 2026 Ibrahim-3d. All Rights Reserved.
This binary distribution is provided for research and evaluation purposes. Unauthorized redistribution or reverse engineering is strictly prohibited.

---
*Developed by Ibrahim-3d | [Report an Issue](https://github.com/Ibrahim-3d/Algorithmic-Zoning-Architect-Dist/issues)*
