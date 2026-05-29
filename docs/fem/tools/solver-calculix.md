# Solver CalculiX

> **In one sentence:** Add a CalculiX solver to the analysis for structural,
> thermal, buckling, frequency, and electromagnetic analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Solver CalculiX

Adds a CalculiX solver object to the analysis. CalculiX (`ccx`) is the most
widely used solver for FEM analysis in FreeCAD. It must be installed
separately and its path set in **Edit → Preferences → FEM → CalculiX**.

---

## Analysis types

Set the **Analysis Type** property to choose what CalculiX computes:

| Analysis Type | What it solves |
|---------------|----------------|
| `static` | Linear or nonlinear static deformation under loads |
| `frequency` | Natural frequencies and mode shapes (modal) |
| `thermomech` | Thermal or thermo-mechanical analysis |
| `buckling` | Linear buckling load factor |
| `check` | Mesh quality check only |
| `electromagnetic` | Electrostatic analysis |

---

## Key properties

| Property | Description |
|----------|-------------|
| Analysis Type | See table above |
| Geometrical Nonlinearity | Enable large-deformation / nonlinear solve |
| Thermomech Type | Coupled / Uncoupled / Pure heat transfer |
| Iterations Control Max | Maximum nonlinear iterations |
| Eigenmode Number | Number of modes (frequency/buckling) |

---

## Common mistakes and pitfalls

!!! warning "CalculiX binary not found"
    Install CalculiX and set the path in **Edit → Preferences → FEM → CalculiX**.
    On Linux, `which ccx` gives the installed path.

!!! warning "Frequency analysis ignores loads"
    Modal analysis computes natural vibration modes — it ignores force and
    pressure loads. Run a separate static analysis for stress under load.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

solver = ObjectsFem.makeSolverCalculiX(doc, "CalculiX")
solver.AnalysisType = "static"
solver.GeometricalNonlinearity = "linear"
analysis.addObject(solver)
doc.recompute()
```

---

## See also

- [Solver Elmer](solver-elmer.md) — multiphysics solver (EM, flow, heat)
- [Run Solver](run-solver.md) — execute the solver
- [Solver Job Control](solver-job-control.md) — monitor output
- [FEM Workbench](../index.md) — workbench overview
