# Self Weight (Gravity)

> **In one sentence:** Apply gravitational acceleration as a body force to
> the entire analysis — 9.81 m/s² downward by default.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Loads → Self Weight (Gravity)

Applies gravitational acceleration as a body force to the whole analysis.
The default direction is −Z (downward) at 9810 mm/s².

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Gravity Acceleration | Magnitude (default 9810 mm/s²) |
| Direction | Usually (0, 0, −1) for downward |

!!! warning "Density must be set"
    Self-weight is proportional to mass. Without a non-zero material density,
    the load is zero.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

grav = ObjectsFem.makeConstraintSelfWeight(doc, "SelfWeight")
grav.Gravity_x = 0.0
grav.Gravity_y = 0.0
grav.Gravity_z = -9.81
analysis.addObject(grav)
doc.recompute()
```

---

## See also

- [Centrifugal Load](load-centrifugal.md) — centrifugal body load
- [Material for Solid](material-solid.md) — density required
- [FEM Workbench](../index.md) — workbench overview
