# Equation: Deformation

> **In one sentence:** Elmer equation for large-strain solid deformation
> using a total Lagrangian formulation — more accurate than Elasticity for
> large-displacement problems.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Deformation

Solves the geometrically nonlinear solid mechanics problem. Uses a total
Lagrangian formulation and supports large strains. More accurate than the
[Elasticity Equation](equation-elasticity.md) for problems with large
displacements (> ~5% strain).

---

## Required boundary conditions

- [Displacement Constraint](constraint-displacement.md) — at minimum
- [Force Load](load-force.md) or [Pressure Load](load-pressure.md)

---

## When to use Deformation vs Elasticity

| | Elasticity | Deformation |
|-|------------|-------------|
| Strain range | Small (< ~5%) | Large (any) |
| Formulation | Linear | Total Lagrangian |
| Computation | Faster | Slower |

---

## See also

- [Equation: Elasticity](equation-elasticity.md) — small-strain linear version
- [Solver Elmer](solver-elmer.md) — must be added to analysis first
- [FEM Workbench](../index.md) — workbench overview
