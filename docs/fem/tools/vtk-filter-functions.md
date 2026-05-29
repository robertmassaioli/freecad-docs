# VTK Create Filter Functions

> **In one sentence:** Defines reusable plane, sphere, or cylinder clip
> geometries that can be referenced by Cut Function and Clip Region filters.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Create Filter Functions

Creates named geometric functions (plane, sphere, or cylinder) that can be
referenced by [Cut Function](vtk-cut-function.md) and [Clip Region](vtk-clip-region.md)
filters. Defining the geometry once here avoids re-entering the same
parameters across multiple filters or pipeline branches.

---

## Function types

| Type | Parameters | Use |
|------|------------|-----|
| Plane | Origin point, normal vector | Planar cross-section cut |
| Sphere | Centre point, radius | Spherical region isolation |
| Cylinder | Centre axis, radius | Cylindrical region or bore cut |

---

## When to use

- When the same clip plane is used in multiple pipeline branches.
- When you want to parameterise the clip geometry (change one definition to
  update all filters that reference it).
- For complex cuts that are tedious to re-enter each time.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Cut Function](vtk-cut-function.md) — apply a clip function to the mesh
- [Clip Region](vtk-clip-region.md) — apply a bounding-box clip
- [FEM Workbench](../index.md) — workbench overview
