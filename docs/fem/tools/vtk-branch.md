# VTK Branch Filter

> **In one sentence:** Splits a VTK pipeline into two independent branches
> so you can display different fields or filters side by side on the same data.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Branch Filter

Duplicates the pipeline at the insertion point, creating two independent
branches that share the same source data but can apply different filters or
display different fields. Useful for comparing stress and displacement on
the same mesh simultaneously, or for applying one cut to two different result
fields.

---

## Use cases

- Display Von Mises stress on the left branch and displacement on the right.
- Apply a Clip Scalar filter to one branch and show the full mesh on the other.
- Compare results before and after a design change by keeping both pipelines
  alive in the same document.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Clip Scalar](vtk-clip-scalar.md) — threshold clip applied to a branch
- [FEM Workbench](../index.md) — workbench overview
