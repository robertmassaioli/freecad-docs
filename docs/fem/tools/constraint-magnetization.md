# Magnetization

> **In one sentence:** Apply a magnetization vector to a body to represent
> a permanent magnet or pre-magnetised region.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Electromagnetic BCs → Magnetization

Applies a magnetization vector to a region of the model, representing a
permanent magnet or pre-magnetised body. Used with the Elmer
[Magnetodynamic Equation](equation-magnetodynamic.md) or Demagnetization equation.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Magnetization vector (x, y, z) | Magnetization components (A/m) |
| References | Body or face to magnetise |

---

## See also

- [Equation: Magnetodynamic](equation-magnetodynamic.md) — the PDE this applies to
- [Electrostatic Potential](constraint-electrostatic-potential.md) — electric potential BC
- [FEM Workbench](../index.md) — workbench overview
