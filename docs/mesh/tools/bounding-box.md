# Bounding Box

> **In one sentence:** Display the axis-aligned bounding box dimensions
> of the selected mesh to check its size and scale.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Analyze → Boundings info  
**Command:** `Mesh_BoundingBox`  
**Shortcut:** none

Displays the axis-aligned bounding box (AABB) of the selected mesh, showing
the minimum and maximum extents in X, Y, and Z and the overall Width, Depth,
and Height.

---

## Intuition

After importing a mesh, the first thing to verify is whether it is at the
correct scale. A human-sized part should be roughly 200 mm tall — not 200
inches. The Bounding Box check takes one second and can save hours of debugging
incorrect print dimensions.

---

## When to use it

- After importing a mesh to confirm the scale matches expectations.
- Before 3-D printing to verify dimensions before slicing.
- Checking that two meshes are at the same scale before Boolean operations.

---

## Reported values

| Value | Description |
|-------|-------------|
| X min / X max | Extent in the X direction (mm) |
| Y min / Y max | Extent in the Y direction (mm) |
| Z min / Z max | Extent in the Z direction (mm) |
| Width | X max − X min |
| Depth | Y max − Y min |
| Height | Z max − Z min |

---

## Common mistakes and pitfalls

!!! warning "Mesh units are always millimetres"
    FreeCAD's internal unit is millimetres. If the bounding box shows the
    wrong scale, the mesh was likely exported in inches or another unit.
    Use [Scale](scale.md) to correct it (e.g., scale by 25.4 for inches → mm).

---

## See also

- [Scale](scale.md) — rescale the mesh if the bounding box reveals a unit error
- [Import Mesh](import-mesh.md) — import meshes to measure
- [Mesh Workbench](../index.md) — workbench overview
