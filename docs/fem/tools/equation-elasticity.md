# Equation: Elasticity

> **In one sentence:** Elmer equation for linear elasticity — standard
> structural stress analysis for small-strain problems.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Elasticity

Solves the linear elasticity PDE (small strain, linear material). This is
the standard structural stress analysis equation for parts where strains
are small (< ~5%).

---

## Required boundary conditions

- [Displacement Constraint](constraint-displacement.md) — at minimum one
- [Force Load](load-force.md) or [Pressure Load](load-pressure.md)

---

## See also

- [Equation: Deformation](equation-deformation.md) — large-strain version
- [Solver Elmer](solver-elmer.md) — must be added first
- [FEM Workbench](../index.md) — workbench overview
