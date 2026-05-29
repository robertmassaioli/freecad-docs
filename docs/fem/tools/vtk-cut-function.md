# VTK Cut Function

> **In one sentence:** Cuts the mesh with a geometric function (plane, sphere,
> or cylinder) to expose internal result values.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Cut Function

Clips the displayed mesh with a geometric surface — a plane, sphere, or
cylinder — to reveal internal field values. Unlike [Clip Scalar](vtk-clip-scalar.md)
(which clips by field value), Cut Function clips by geometry regardless of
field magnitude.

---

## Common uses

- Cross-section through the centre of a pressure vessel to see internal
  stress distribution.
- Axial cut through a rotating component to see the radial temperature
  variation.
- Spherical clip to isolate a localised region of interest.

---

## Clip functions

Clip functions are defined with [Create Filter Functions](vtk-filter-functions.md).
You can save a plane, sphere, or cylinder definition once and reuse it
across multiple Cut Function and Clip Region filters.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Create Filter Functions](vtk-filter-functions.md) — define reusable clip geometries
- [Clip Scalar](vtk-clip-scalar.md) — clip by field value instead
- [Clip Region](vtk-clip-region.md) — clip by bounding box
- [FEM Workbench](../index.md) — workbench overview
