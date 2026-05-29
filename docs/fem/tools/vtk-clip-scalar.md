# VTK Clip Scalar

> **In one sentence:** Hides all mesh elements where a chosen scalar result
> field exceeds (or falls below) a threshold, revealing only the region of interest.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Clip Scalar

Removes from the display every element whose scalar result value lies
outside the specified threshold. Used to isolate regions where stress,
temperature, or any other scalar exceeds a critical limit.

---

## Use cases

- **Yield zone mapping:** Set scalar = Von Mises Stress, threshold = material
  yield stress. Elements above the threshold disappear, revealing only the
  plastically-loaded region.
- **Hot spot isolation:** Set scalar = Temperature, threshold = maximum
  allowable temperature to see where the thermal limit is exceeded.
- **Deflection check:** Clip by displacement magnitude to highlight the most
  displaced region.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Cut Function](vtk-cut-function.md) — clip by geometry instead of field value
- [Clip Region](vtk-clip-region.md) — clip by bounding box
- [FEM Workbench](../index.md) — workbench overview
