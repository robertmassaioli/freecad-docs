# VTK Warp by Vector

> **In one sentence:** Deforms the displayed mesh by adding a scaled vector
> result field to each node position, showing the deformed shape.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Warp by Vector

Offsets each node of the displayed mesh by the vector result field (typically
Displacement) multiplied by a scale factor. Used to visualise structural
deformation — the mesh shape reflects the deflected geometry.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Vector field | The vector result to warp by (usually Displacement) |
| Warp factor | Scale applied to the displacement (e.g. 100×) |

---

## Intuition

A **warp factor of 1.0** shows the true deformed shape at 1:1 scale. For
typical elastic deformations (0.01–1 mm on a 100 mm part), a factor of
100–10 000 is needed to make the deflection visible. The colour map on the
warped mesh shows the field magnitude — stress or displacement — at the
(scaled) deformed position.

---

## Common mistakes and pitfalls

!!! warning "Warp factor distorts apparent magnitudes"
    A warp factor of 1000 makes a 0.01 mm deflection look like 10 mm. Never
    report deformation magnitude from a screenshot — read the actual value from
    [Show Results](results-show.md) or [Data at Point](vtk-data-at-point.md).

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Data at Point](vtk-data-at-point.md) — read the actual displacement value
- [Show Results](results-show.md) — simpler colour map without warp
- [FEM Workbench](../index.md) — workbench overview
