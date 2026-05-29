# Initial Flow Velocity

> **In one sentence:** Set the uniform starting velocity field for a transient
> fluid flow analysis or provide an initial guess for steady-state convergence.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Fluid BCs → Initial Flow Velocity

Sets the uniform starting velocity field for a transient (time-dependent)
fluid flow analysis using Elmer. At t = 0, every point in the fluid domain
has this velocity. For steady-state flow, it serves as an initial guess for
the iterative Navier-Stokes solver.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Velocity (x, y, z) | Initial velocity vector (m/s) |
| Enable component | Toggle each velocity component independently |

---

## See also

- [Initial Pressure](initial-pressure.md) — starting pressure for transient flow
- [Flow Velocity Constraint](constraint-flow-velocity.md) — inlet/wall velocity BC
- [Equation: Flow](equation-flow.md) — Elmer Navier-Stokes equation
- [FEM Workbench](../index.md) — workbench overview
