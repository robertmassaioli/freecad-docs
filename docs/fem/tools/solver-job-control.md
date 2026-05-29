# Solver Job Control

> **In one sentence:** Open the solver settings panel and real-time output
> monitor — inspect the input file, monitor the run, and re-read results.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Solver Job Control

Opens the solver job control panel — a combined settings dialog and output
monitor. From here you can view and edit solver settings, inspect the
generated input file, monitor the solver log in real time, and re-read
results from a previous run.

---

## What you can do

- View and edit the solver's working directory.
- Open the generated solver input file (e.g. `.inp` for CalculiX) for inspection or editing.
- Monitor the solver output log in real time.
- Re-read results from a previously completed run without re-running.

---

## Inspecting CalculiX input files

The CalculiX `.inp` file in the working directory lets you:
- Verify boundary conditions, material properties, and step definitions.
- Add CalculiX keywords not exposed in the FreeCAD UI.
- Debug unexpected solver failures.

---

## See also

- [Run Solver](run-solver.md) — execute the solver
- [Solver CalculiX](solver-calculix.md) — the solver whose input files are inspected here
- [FEM Workbench](../index.md) — workbench overview
