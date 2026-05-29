# Evaluate Facet

> **In one sentence:** Click any triangle on a mesh to display its index,
> area, normal, aspect ratio, and neighbour information.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Analyze → Evaluate Facet  
**Command:** `Mesh_EvaluateFacet`  
**Shortcut:** none

Activates a pick mode — click on any triangle in the mesh to display its
properties in the Task panel.

---

## Intuition

When you see a visual artifact or suspect a degenerate triangle in a specific
mesh region, Evaluate Facet lets you click exactly that triangle to read its
numerical properties — area, normal direction, neighbours — so you can decide
whether it needs repair.

---

## When to use it

- Finding degenerate triangles with near-zero area.
- Checking the normal direction at a specific location.
- Investigating a region that appears wrong in the vertex curvature colour map.
- Identifying which triangle index to reference in a repair script.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Analyze → Evaluate Facet**.
3. Click any triangle in the 3-D view.
4. Read the properties in the Task panel.
5. Press **Escape** or click **Close** to exit.

---

## Reported properties

| Property | Description |
|----------|-------------|
| Face index | Triangle index in the mesh data structure |
| Triangle area | Area of the triangle (mm²) |
| Normal | X, Y, Z components of the outward normal |
| Aspect ratio | Longest edge / shortest altitude (quality metric; 1.0 = equilateral) |
| Neighbours | Indices of adjacent triangles |
| Neighbours' normals | Normal vectors of adjacent triangles |

---

## Common mistakes and pitfalls

!!! warning "Aspect ratio interpretation"
    An aspect ratio of 1.0 is a perfect equilateral triangle. Ratios above
    10–20 indicate very elongated triangles that may cause issues in FEA
    meshing. Ratios near infinity indicate degenerate (zero-area) triangles.

---

## See also

- [Evaluate and Repair](evaluate-and-repair.md) — fix the defects you find
- [Vertex Curvature](vertex-curvature.md) — colour map to locate suspect regions
- [Mesh Workbench](../index.md) — workbench overview
