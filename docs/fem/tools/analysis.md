# Analysis Container

> **In one sentence:** The Analysis container is the root object that groups
> all FEM study objects — materials, boundary conditions, mesh, solver, and
> results — and must be created before anything else in the FEM workbench.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Analysis Container  
**Shortcut:** `S`, `A`

---

## Intuition

The Analysis container (`Fem::FemAnalysis`) is to FEM what the Assembly
container is to the Assembly workbench — a logical boundary that separates
one study from another. Inside it lives everything the solver needs:

```
Analysis
├── Material_Solid          ← material properties
├── Constraint_Fixed        ← boundary condition
├── Constraint_Force        ← load
├── FEMMesh_Netgen          ← mesh
├── SolverCalculiX          ← solver configuration
└── ResultMechanical        ← results (after solve)
```

You can have multiple analyses in one document — a static analysis and a
frequency (modal) analysis of the same geometry, for example. Each analysis
is self-contained: switching the active analysis does not affect the others.

The active analysis is shown in **bold** in the model tree. New objects
(materials, constraints, mesh) are always added to the active analysis.

---

## Creating an analysis

1. Choose **FEM → Model → Analysis Container** (or press `S` then `A`).
2. The Analysis container appears in the model tree and is automatically
   activated.
3. Proceed to add a material, boundary conditions, mesh, and solver.

---

## Activating an analysis

To switch the active analysis, **double-click** it in the model tree.
The active analysis is highlighted and shown in bold. All subsequent
FEM tool operations apply to the active analysis.

---

## Multiple analyses

A common pattern is to use multiple analyses for different load cases:

```
Document
├── Body (Part Design geometry)
├── Analysis_Static
│   ├── Material_Steel
│   ├── Constraint_Fixed (base face)
│   ├── Constraint_Force (top face, Z direction)
│   └── SolverCalculiX (static)
└── Analysis_Frequency
    ├── Material_Steel           ← same material, different analysis
    ├── Constraint_Fixed (base face)
    └── SolverCalculiX (frequency / modal)
```

Each analysis has its own solver, its own result objects, and can have
its own set of boundary conditions. The geometry and mesh can be shared
across analyses (insert the same mesh object into both).

---

## Python API

```python
import FreeCAD as App
import ObjectsFem

doc = App.newDocument()

# Create an analysis container
analysis = ObjectsFem.makeAnalysis(doc, "Analysis")
doc.recompute()

# The analysis is now the active analysis.
# Add a solver:
solver = ObjectsFem.makeSolverCalculiX(doc, "CalculiX")
analysis.addObject(solver)
doc.recompute()
```

---

## See also

- Materials — assign material properties to the body inside the analysis
- Mesh from Shape — generate the finite element mesh
- Solver CalculiX — configure and run the static or thermal solver
- Show Results — display the computed result fields
