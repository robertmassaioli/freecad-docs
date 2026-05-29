# Displacement Constraint

> **In one sentence:** Prescribe the displacement or rotation of a face, edge,
> or vertex to a specific value — including zero (fixed in that direction) or
> a non-zero prescribed deformation.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions → Displacement Constraint

Prescribes the displacement (and/or rotation) of a geometric entity. Each
of the six DOF can independently be free, fixed at zero, or set to a
prescribed non-zero value.

---

## Parameters

| DOF | Meaning | Example use |
|-----|---------|-------------|
| x | Translation in X | Fix X only — allows Y, Z sliding |
| y | Translation in Y | — |
| z | Translation in Z | Fix Z only — roller support |
| Rx | Rotation about X | Fix rotation — pinned joint |
| Ry | Rotation about Y | — |
| Rz | Rotation about Z | — |

**Free** — not constrained  
**Fixed (= 0)** — zero displacement/rotation  
**Prescribed** — specific non-zero value  

---

## Example: Roller support

- x: Free (slides)
- y: Fixed (0)
- z: Fixed (0)
- Rx, Ry, Rz: Free

---

## See also

- [Fixed Constraint](constraint-fixed.md) — fix all six DOF at once
- [Transform Constraint](constraint-transform.md) — apply in a local coordinate system
- [FEM Workbench](../index.md) — workbench overview
