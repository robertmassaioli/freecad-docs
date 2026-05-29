# Fixed Constraint

> **In one sentence:** Prevent all translational and rotational motion on a
> selected face, edge, or vertex — the most common structural boundary condition.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions → Fixed Constraint

Prevents all six degrees of freedom (3 translations + 3 rotations) from
moving on the selected geometry. This is the simplest constraint and the
most commonly used.

---

## When to use Fixed vs Displacement

| Situation | Use |
|-----------|-----|
| The surface truly cannot move at all (bolted base, welded joint to a rigid wall) | Fixed |
| Some motion is allowed (e.g. can slide in one direction) | [Displacement Constraint](constraint-displacement.md) |
| A known non-zero deformation must be applied | [Displacement Constraint](constraint-displacement.md) |

---

## Step-by-step

1. Select the face, edge, or vertex to constrain.
2. Choose **FEM → Model → Mechanical BCs → Fixed Constraint**.
3. Click **OK**. A visual indicator appears on the geometry.

---

## Common mistakes and pitfalls

!!! warning "No constraint → singular stiffness matrix"
    Every structural analysis must have at least one Fixed or Displacement
    constraint to prevent rigid body motion. Without it the solver fails with
    a singular matrix error.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

fix = ObjectsFem.makeConstraintFixed(doc, "ConstraintFixed")
fix.References = [(doc.getObject("Body"), "Face1")]
analysis.addObject(fix)
doc.recompute()
```

---

## See also

- [Displacement Constraint](constraint-displacement.md) — prescribe specific displacements
- [Force Load](load-force.md) — apply a force load
- [FEM Workbench](../index.md) — workbench overview
