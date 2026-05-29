# Vertex Curvature

> **In one sentence:** Create a colour-map object showing surface curvature
> at every vertex of the mesh — blue for flat regions, red for sharp edges
> and corners.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Vertex Curvature  
**Command:** `Mesh_VertexCurvature`  
**Shortcut:** none

Creates a `Mesh::Curvature` object in the model tree that stores principal
curvature values at every vertex. The mesh is displayed with a colour map
indicating curvature variation across the surface.

---

## Intuition

Curvature measures how sharply a surface bends. Flat faces have zero curvature;
edges and corners have high curvature. The colour map makes it immediately
obvious where a mesh has fine detail (red), smooth transitions (green), or
flat regions (blue).

Vertex Curvature is also a prerequisite for [Curvature Info](curvature-info.md),
which lets you click a point to read the exact curvature value there.

---

## When to use it

- Identifying sharp features and corners in a scanned mesh.
- Locating degenerate triangles (isolated red dots on a nominally flat face
  often indicate noise or bad topology).
- As a prerequisite for [Curvature Info](curvature-info.md) to read numerical
  values at specific points.
- Validating surface quality before reverse-engineering fit operations.

---

## Step-by-step

1. Select the mesh in the model tree.
2. Choose **Mesh → Meshes → Vertex Curvature**.
3. A `Mesh::Curvature` object appears in the tree.
4. The mesh is now displayed with the curvature colour map.
5. Use [Curvature Info](curvature-info.md) to click individual vertices for
   numerical readings.

---

## Colour map reference

| Colour | Curvature meaning |
|--------|------------------|
| Blue | Low curvature (flat regions) |
| Green | Moderate curvature |
| Red | High curvature (sharp edges, corners) |

---

## Common mistakes and pitfalls

!!! warning "Isolated red dots on flat faces indicate noise"
    A flat face should show uniform blue. Isolated red spots usually indicate
    degenerate triangles, non-manifold points, or scan noise. Run
    [Evaluate and Repair](evaluate-and-repair.md) to fix the underlying
    topology issues.

---

## Python API

```python
import FreeCAD as App
import Mesh

doc = App.ActiveDocument
mesh = doc.getObject("Mesh")

curvature = doc.addObject("Mesh::Curvature", "MeshCurvature")
curvature.Source = mesh
doc.recompute()
```

---

## See also

- [Curvature Info](curvature-info.md) — read numerical curvature at a clicked point
- [Evaluate and Repair](evaluate-and-repair.md) — fix topology issues revealed by the map
- [Segmentation (Curvature)](segmentation-curvature.md) — use curvature to segment the mesh
- [Mesh Workbench](../index.md) — workbench overview
