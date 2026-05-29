# Equation: Electrostatic

> **In one sentence:** Elmer equation for electric potential and field —
> solves the Laplace/Poisson equation ∇·(ε∇V) = −ρ_f.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Electrostatic

Solves the Laplace/Poisson equation for the electric potential, then derives
the electric field **E** = −∇V.

---

## Required

- **BCs:** [Electrostatic Potential](constraint-electrostatic-potential.md) on conductors
- **Material:** Dielectric permittivity (relative permittivity ε_r)

---

## Use cases

Capacitor fields, insulation analysis, electrostatic actuators, electrode
field mapping.

---

## See also

- [Constraint: Electrostatic Potential](constraint-electrostatic-potential.md)
- [Equation: Static Current](equation-static-current.md) — DC conduction current
- [Solver Elmer](solver-elmer.md) — must be added first
- [FEM Workbench](../index.md) — workbench overview
