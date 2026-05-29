# Equation: Heat

> **In one sentence:** Elmer equation for heat conduction and convection —
> solves ρc(∂T/∂t) = ∇·(λ∇T) + Q for the temperature field.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Heat

Solves the heat conduction-convection equation for steady-state or transient
temperature distribution.

---

## Required

- **BCs:** [Temperature Constraint](constraint-temperature.md) (Dirichlet) and/or [Heat Flux Load](load-heat-flux.md) (Neumann)
- **Material:** Thermal conductivity (λ), specific heat (c), density (ρ)

---

## Use cases

Steady-state temperature distribution, transient cool-down, conjugate heat
transfer (coupled with the [Flow Equation](equation-flow.md)).

---

## See also

- [Equation: Flow](equation-flow.md) — couple with Heat for conjugate heat transfer
- [Constraint: Temperature](constraint-temperature.md) — temperature BC
- [Load: Heat Flux](load-heat-flux.md) — surface heat flux BC
- [Solver Elmer](solver-elmer.md) — must be added first
- [FEM Workbench](../index.md) — workbench overview
