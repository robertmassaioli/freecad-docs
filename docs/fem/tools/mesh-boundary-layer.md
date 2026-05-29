# FEM Mesh Boundary Layer

> **In one sentence:** Add prismatic boundary layer elements adjacent to
> selected wall faces in a Gmsh mesh for fluid flow analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh → FEM Mesh Boundary Layer

Adds a prismatic boundary layer stack to selected faces in a Gmsh mesh.
Boundary layers are thin, structured layers essential for CFD analysis to
resolve the velocity gradient in the viscous sublayer.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Start Size | Thickness of the first layer (closest to wall) |
| Growth Rate | Ratio between successive layer thicknesses (e.g. 1.2) |
| Number of Layers | Total number of prismatic layers |
| References | Faces to apply the boundary layer to |

---

## Choosing parameters

For turbulent flow at moderate Reynolds numbers:
- First layer at y⁺ ≈ 1 (viscous sublayer) or y⁺ ≈ 30–100 (log-law, wall functions).
- Growth rate of 1.2–1.3 is typical.
- 10–20 layers is common.

---

## Common mistakes and pitfalls

!!! warning "Gmsh only"
    Boundary layers are a Gmsh feature and cannot be added to Netgen meshes.

---

## See also

- [Mesh from Shape (Gmsh)](mesh-gmsh.md) — required base mesh
- [Equation: Flow](equation-flow.md) — the Elmer flow equation that needs boundary layers
- [FEM Workbench](../index.md) — workbench overview
