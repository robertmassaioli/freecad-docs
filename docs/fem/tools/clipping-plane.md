# Clipping Plane

> **In one sentence:** Adds (or removes) an interactive clipping plane in the
> 3-D viewport to cut through the model and inspect internal geometry.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Utilities → Add Clipping Plane / Remove All Clipping Planes

The clipping plane is a viewport-level display tool — it cuts the visual
display of the 3-D model (mesh, solid) along an infinite plane so you can
see inside. This is independent of the VTK [Cut Function](vtk-cut-function.md),
which clips result data in the post-processing pipeline.

---

## Add Clipping Plane

**Menu:** FEM → Utilities → Add Clipping Plane

Adds one interactive clipping plane to the 3-D view. You can move and tilt
the plane by dragging its handle in the viewport. Multiple clipping planes
can be added — each successive call adds another plane.

---

## Remove All Clipping Planes

**Menu:** FEM → Utilities → Remove All Clipping Planes

Removes every active clipping plane from the 3-D view, restoring the full
solid display.

---

## Clipping plane vs VTK Cut Function

| Feature | Clipping Plane | VTK Cut Function |
|---------|----------------|-----------------|
| Affects | Visual display only | Result field data |
| Requires VTK? | No | Yes |
| Shows result values? | No | Yes |
| Available before solving? | Yes | No |

---

## See also

- [VTK Cut Function](vtk-cut-function.md) — cut result data in the VTK pipeline
- [FEM Workbench](../index.md) — workbench overview
