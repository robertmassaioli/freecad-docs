# Equation: Static Current

> **In one sentence:** Elmer equation for DC electric conduction —
> solves ∇·(σ∇V) = 0 for current density and Joule heating.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Static Current

Solves for the DC electric current distribution in a conductor. Computes
current density **J** = σ∇V and Joule heating (I²R losses).

---

## Required

- **BCs:** [Electrostatic Potential](constraint-electrostatic-potential.md) on terminals
- **Material:** Electrical conductivity (σ)

---

## Use cases

Busbar current distribution, PCB trace heating, grounding electrode design.

---

## See also

- [Equation: Electrostatic](equation-electrostatic.md) — for capacitor / insulator problems
- [Constraint: Electrostatic Potential](constraint-electrostatic-potential.md) — voltage BC
- [Solver Elmer](solver-elmer.md) — must be added first
- [FEM Workbench](../index.md) — workbench overview
