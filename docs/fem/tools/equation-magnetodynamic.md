# Equation: Magnetodynamic

> **In one sentence:** Elmer equation for time-harmonic (AC) electromagnetic
> fields — computes magnetic field, eddy currents, and Joule heating.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Solve → Elmer Equations → Magnetodynamic

Solves the time-harmonic (AC, frequency-domain) Maxwell equations for the
magnetic vector potential **A**. Computes magnetic field, eddy currents,
and Joule heating in conductors.

---

## Required

- **BCs:** [Magnetization](constraint-magnetization.md) or [Current Density](constraint-current-density.md) on source regions
- **Material:** Electrical conductivity, relative permeability

---

## Use cases

Induction heating, transformer core analysis, eddy current braking, wireless
power transfer.

---

## See also

- [Equation: Magnetodynamic 2D](equation-magnetodynamic-2d.md) — 2-D planar/axisymmetric version
- [Constraint: Magnetization](constraint-magnetization.md) — permanent magnet BC
- [Constraint: Current Density](constraint-current-density.md) — current source BC
- [Solver Elmer](solver-elmer.md) — must be added first
- [FEM Workbench](../index.md) — workbench overview
