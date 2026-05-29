# VTK Clip Region

> **In one sentence:** Clips the displayed mesh to a rectangular bounding box,
> isolating a specific zone of the geometry for inspection.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Clip Region

Restricts the visible mesh to the elements inside a user-defined bounding
box. A simpler alternative to [Cut Function](vtk-cut-function.md) when you
only need to isolate a rectangular zone — a corner, a joint, or a specific
section of the model.

---

## When to use

- Isolating one corner of a structure to inspect a stress concentration.
- Focusing on a single weld zone in a large assembly result.
- Quickly checking an internal region without defining a precise cut plane.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Cut Function](vtk-cut-function.md) — clip by plane, sphere, or cylinder
- [Clip Scalar](vtk-clip-scalar.md) — clip by field value
- [Create Filter Functions](vtk-filter-functions.md) — define reusable clip geometries
- [FEM Workbench](../index.md) — workbench overview
