# Solver Elmer

> **In one sentence:** Add an Elmer multiphysics solver to the analysis —
> define what physics to solve by adding equation objects alongside it.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Solver Elmer

Adds an Elmer solver object to the analysis. Elmer is an open-source
multiphysics solver for electrostatics, electromagnetics, fluid flow
(Navier-Stokes), heat conduction, and solid deformation. It must be installed
and configured in **Edit → Preferences → FEM → Elmer**.

Unlike CalculiX, Elmer has no built-in analysis type — you define what to
solve by adding **equation objects** (e.g. [Heat Equation](equation-heat.md),
[Flow Equation](equation-flow.md)) to the analysis container.

---

## Elmer workflow

1. Add **Solver Elmer** to the analysis.
2. Add one or more **equation objects**.
3. Add materials with properties matching the equations.
4. Add boundary conditions matching the equations.
5. Run the solver.

---

## Key properties

| Property | Description |
|----------|-------------|
| Simulation Type | Steady State / Transient / Scanning |
| Time Step Intervals | Number of time steps (transient) |
| Timestep Size | Size of each time step (s) |
| Output Frequency | Write results every N steps |

---

## Common mistakes and pitfalls

!!! warning "Equations without Elmer solver"
    Elmer equation objects are meaningless without a Solver Elmer object in
    the analysis. Add Solver Elmer before adding equations.

!!! warning "Elmer requires Gmsh mesh groups"
    Elmer identifies boundary faces by group names. Use Gmsh + FEM Mesh Group
    — not Netgen — for all Elmer analyses.

---

## See also

- [Solver CalculiX](solver-calculix.md) — structural/thermal solver
- [Equation: Heat](equation-heat.md) — thermal equation for Elmer
- [Equation: Flow](equation-flow.md) — Navier-Stokes for Elmer
- [FEM Mesh Group](mesh-group.md) — required for Elmer boundary conditions
- [FEM Workbench](../index.md) — workbench overview
