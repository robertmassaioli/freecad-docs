# Force Load

> **In one sentence:** Apply a total concentrated or distributed force to
> a face, edge, or vertex in a specified direction.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Loads → Force Load

Applies a total force distributed over the selected geometry. The force is
distributed across the selected face, edge, or vertex.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Force | Total force magnitude (N) |
| Direction | Force vector direction (X, Y, Z components or selected edge/face normal) |
| References | Face(s), edge(s), or vertex/vertices to load |
| Reversed | Flip the direction of the force |

---

## Force Load vs Pressure Load

- **Force Load** — total force regardless of selected face area. A larger
  face gets a lower surface pressure for the same total force.
- **[Pressure Load](load-pressure.md)** — force per unit area; total force scales
  with face area.

Use Force when you know the total applied force (e.g. bolt pretension = 5000 N).
Use Pressure when you know the surface pressure (e.g. hydraulic fluid at 10 bar).

---

## Common mistakes and pitfalls

!!! warning "Force direction varies on curved faces"
    If the direction is set to the face normal and the face is curved, the force
    direction varies across the face. Set an explicit global direction vector for
    most concentrated force loads.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

force = ObjectsFem.makeConstraintForce(doc, "Force")
force.Force = 1000.0  # N
force.References = [(doc.getObject("Body"), "Face5")]
analysis.addObject(force)
doc.recompute()
```

---

## See also

- [Pressure Load](load-pressure.md) — apply force per unit area
- [Self Weight (Gravity)](load-self-weight.md) — gravitational body load
- [Fixed Constraint](constraint-fixed.md) — reaction boundary condition
- [FEM Workbench](../index.md) — workbench overview
