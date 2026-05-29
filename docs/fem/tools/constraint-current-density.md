# Current Density

> **In one sentence:** Prescribe the electric current density (A/m²) as a
> Neumann boundary condition on a face.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Electromagnetic BCs → Current Density

Prescribes the electric current density on a face — the electromagnetic
equivalent of a heat flux load. Use when the current flowing through a
surface is known rather than the voltage.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Current density (x, y, z) | Current density vector components (A/m²) |
| References | Face to carry the prescribed current |

---

## See also

- [Electrostatic Potential](constraint-electrostatic-potential.md) — Dirichlet EM BC
- [Equation: Electrostatic](equation-electrostatic.md) — the PDE this BC applies to
- [FEM Workbench](../index.md) — workbench overview
