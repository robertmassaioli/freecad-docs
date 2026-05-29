# Rigid Body Constraint

> **In one sentence:** Constrain all nodes on a face to move together as a
> single rigid body, preventing relative deformation between them.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions → Rigid Body Constraint

Constrains all nodes on a face to move as one rigid body — no relative
deformation within the face. The face as a whole can still translate and
rotate (unless other constraints prevent it).

---

## When to use it

- **Load distribution:** Apply a force or moment to a face that should be
  distributed realistically without the face itself deforming — avoids stress
  concentrations at load application points.
- **Stiff attachment:** Model a stiff connector or bracket face that does not flex.

---

## Reference point

Rigid body motion is tied to a **reference point** (a node or defined point)
that carries the resultant force and moment. The solver computes nodal forces
on the face consistent with the applied load at the reference point.

---

## See also

- [Fixed Constraint](constraint-fixed.md) — fix all DOF
- [Displacement Constraint](constraint-displacement.md) — prescribe specific DOF
- [Force Load](load-force.md) — the load typically applied through a rigid body
- [FEM Workbench](../index.md) — workbench overview
