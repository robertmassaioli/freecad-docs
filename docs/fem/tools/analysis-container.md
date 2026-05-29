# Analysis Container

> **In one sentence:** The root container object for an FEM study — all
> materials, boundary conditions, mesh, solver, and results live inside it.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Analysis Container  
**Shortcut:** `S`, `A`

The Analysis container (`Fem::FemAnalysis`) is the logical boundary of one
FEM study. Everything the solver needs — materials, mesh, boundary conditions,
solver configuration, and results — lives inside it.

---

## Intuition

```
Analysis
├── Material_Solid          ← material properties
├── Constraint_Fixed        ← boundary condition
├── Constraint_Force        ← load
├── FEMMesh_Netgen          ← mesh
├── SolverCalculiX          ← solver configuration
└── ResultMechanical        ← results (after solve)
```

You can have multiple analyses in one document — for example a static analysis
and a frequency (modal) analysis of the same geometry. Each is self-contained.
The active analysis is shown in **bold** in the model tree; all new FEM objects
are added to the active analysis.

---

## Step-by-step

1. Choose **FEM → Model → Analysis Container** (or press `S`, `A`).
2. The Analysis container appears and is automatically activated.
3. Add materials, boundary conditions, mesh, and a solver in sequence.

## Activating an analysis

Double-click an analysis in the model tree to make it the active analysis.

---

## Python API

```python
import FreeCAD as App
import ObjectsFem

doc = App.newDocument()
analysis = ObjectsFem.makeAnalysis(doc, "Analysis")
doc.recompute()
```

---

## See also

- [Material for Solid](material-solid.md) — assign material properties
- [Mesh from Shape (Netgen)](mesh-netgen.md) — generate the mesh
- [Solver CalculiX](solver-calculix.md) — configure and run the solver
- [Show Results](results-show.md) — display computed result fields
- [FEM Workbench](../index.md) — workbench overview
