# Polar Array

> **In one sentence:** Creates copies arranged evenly in a ring (or arc)
> around a centre point and axis.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools → Polar Array  
**Command:** `Draft_PolarArray`

---

## Parameters

| Property | Description |
|----------|-------------|
| Number Polar | Number of copies (including original) |
| Polar Angle | Total arc swept (360° for a full ring) |
| Center | Centre of rotation |
| Axis | Rotation axis (default: Z) |

---

## Python API

```python
import Draft

bolt = App.ActiveDocument.getObject("Body")
arr = Draft.make_polar_array(
    obj=bolt,
    number=6,
    angle=360,
    center=App.Vector(0, 0, 0),
    use_link=True
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Polar Angle is total swept angle, not step angle"
    With Number=6 and Polar Angle=360°, copies are at 0°, 60°, 120°, 180°,
    240°, 300°. Setting Polar Angle=60° with Number=6 packs 6 copies into
    60°, spaced 12° apart.

---

## See also

- [Array Ortho](array-ortho.md) — rectangular grid
- [Array Circular](array-circular.md) — concentric rings
- [Draft Workbench](../index.md) — workbench overview
