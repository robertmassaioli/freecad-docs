# VTK Data at Point

> **In one sentence:** Reads the result field value at a single point by
> finding the nearest mesh node and reporting its value.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Data at Point

Places a probe at a user-specified coordinate and returns the interpolated
result field value at that location. Useful for spot-checking a value at a
known location, verifying against an analytical solution, or reporting the
result at a specific design point.

---

## Use cases

- **Verification:** Check the displacement at the free end of a cantilever
  beam against the analytical formula δ = FL³ / 3EI.
- **Design check:** Confirm that the temperature at a sensor location stays
  below a limit.
- **Report generation:** Extract exact values at specified points for a
  design report.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Data Along Line](vtk-data-along-line.md) — sample values along a full line
- [Show Results](results-show.md) — colour map to locate regions of interest
- [FEM Workbench](../index.md) — workbench overview
