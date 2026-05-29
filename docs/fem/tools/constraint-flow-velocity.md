# Flow Velocity Constraint

> **In one sentence:** Prescribe the fluid velocity on inlet, wall, and
> symmetry faces for Elmer Navier-Stokes analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Fluid BCs → Flow Velocity Constraint

Prescribes the fluid velocity on selected faces. This is the primary inlet,
outlet, and wall boundary condition for Elmer Navier-Stokes flow analysis.

---

## Common configurations

| Face type | Velocity prescription |
|-----------|----------------------|
| Inlet | Set all velocity components (uniform or profile) |
| Wall (no-slip) | All components = 0 |
| Slip wall | Normal component = 0, tangential components free |
| Outlet | Often left unconstrained (natural outflow) |
| Symmetry plane | Normal component = 0 |

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Velocity x | X-component of velocity (m/s) or free |
| Velocity y | Y-component |
| Velocity z | Z-component |
| Normalise | Normalise to a unit inlet profile |

---

## Common mistakes and pitfalls

!!! warning "Inlet velocity must be physically consistent"
    For incompressible flow, the inlet flow must balance the outlet flow
    (continuity ∇·v = 0). An inconsistent inlet with no matching outlet
    BC can cause the solver to diverge.

---

## See also

- [Initial Flow Velocity](initial-flow-velocity.md) — starting velocity field
- [Equation: Flow](equation-flow.md) — Elmer Navier-Stokes equation
- [FEM Workbench](../index.md) — workbench overview
