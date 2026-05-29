# Run Solver

> **In one sentence:** Write input files, launch the external solver, and
> read results back into FreeCAD — the single "go" button for FEM analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Run Solver

Writes the solver input files, launches the external solver executable, and
reads the result files when the run finishes. Result objects appear in the
analysis tree after a successful run.

---

## What happens

1. FreeCAD writes the solver input files to the working directory.
2. The external solver executable is launched.
3. When the solver finishes, FreeCAD reads the output files.
4. Result objects (`ResultMechanical`, etc.) appear in the analysis tree.

If the solver fails, check the output log in [Solver Job Control](solver-job-control.md).

---

## Common mistakes and pitfalls

!!! warning "Solver binary not found"
    The solver must be installed and its path set in FreeCAD Preferences.
    See [Solver CalculiX](solver-calculix.md) or [Solver Elmer](solver-elmer.md)
    for setup instructions.

---

## See also

- [Solver Job Control](solver-job-control.md) — monitor output and inspect input files
- [Show Results](results-show.md) — display results after a successful run
- [FEM Workbench](../index.md) — workbench overview
