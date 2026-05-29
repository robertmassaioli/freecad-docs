# Purge Results

> **In one sentence:** Deletes all result and VTK pipeline objects from the
> active analysis to free memory before re-running the solver.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Purge Results

Removes every `ResultMechanical` object and VTK pipeline from the active
analysis container. Use before re-running a solver if you want to start
from a clean state, or to reduce document file size after reviewing results.

---

## When to use

- Before re-running a solver after changing loads or mesh to avoid confusion
  between old and new results.
- To free memory when working with large result arrays.
- To clean up the model tree before sharing or archiving the document.

---

## Common mistakes and pitfalls

!!! warning "Purge is not undoable"
    Purge Results permanently deletes all result data from the document.
    There is no undo. Save the document first if you may need the results later.

---

## See also

- [Show Results](results-show.md) — display a result field on the mesh
- [Run Solver](run-solver.md) — execute the solver to generate new results
- [FEM Workbench](../index.md) — workbench overview
