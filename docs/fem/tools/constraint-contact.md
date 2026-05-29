# Contact Constraint

> **In one sentence:** Define a contact pair between two faces — the solver
> prevents them from overlapping and optionally enforces friction.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions → Contact Constraint

Defines a contact pair between two faces that may touch, separate, or slide
relative to each other during the analysis. The solver enforces non-penetration
and optionally applies a friction coefficient.

---

## Contact types

| Type | Penetration | Separation | Friction |
|------|-------------|------------|---------|
| Frictionless | Prevented | Allowed | No |
| Rough (no slip) | Prevented | Allowed | Full stick |
| Coulomb friction | Prevented | Allowed | μ coefficient |

---

## Step-by-step

1. Select the **master face** (usually the stiffer body).
2. Ctrl-click the **slave face** (usually the more compliant body).
3. Choose **FEM → Model → Mechanical BCs → Contact Constraint**.
4. Set the friction coefficient (0 for frictionless).
5. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Contact requires a nonlinear solver"
    Contact is nonlinear — the contact area changes with load. CalculiX must
    run with **Geometrical Nonlinearity** enabled. Linear static mode does
    not support contact.

---

## See also

- [Tie Constraint](constraint-tie.md) — bond faces permanently (no separation)
- [Solver CalculiX](solver-calculix.md) — must be in nonlinear mode for contact
- [FEM Workbench](../index.md) — workbench overview
