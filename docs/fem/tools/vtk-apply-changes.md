# VTK Apply Changes

> **In one sentence:** Forces all VTK filters in the pipeline to update
> when the display appears stale after editing filter parameters.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Apply Changes

Normally, changes to VTK filter parameters update the display automatically.
Use **Apply Changes** when the visualisation appears stale — for example,
after editing a Clip Scalar threshold or a Calculator expression and the
mesh display has not refreshed.

---

## When to use

- After editing filter parameters if the mesh display does not update.
- After switching the result field on the pipeline.
- When chained filters in a complex pipeline lose synchronisation.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [FEM Workbench](../index.md) — workbench overview
