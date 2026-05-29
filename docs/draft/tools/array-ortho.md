# Ortho Array

> **In one sentence:** Creates a 3-D rectangular grid of copies along up to
> three orthogonal axes, each with independent count and spacing.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools → Ortho Array  
**Command:** `Draft_OrthoArray`

Total elements = Number X × Number Y × Number Z. The Interval is
**centre-to-centre** distance, not the gap.

---

## Parameters

| Property | Description |
|----------|-------------|
| Number X / Y / Z | Count along each axis (includes original) |
| Interval X / Y / Z | Spacing vector in each direction |
| Fuse | Fuse all elements into one shape |

---

## Python API

```python
import Draft

bolt = App.ActiveDocument.getObject("Body")
arr = Draft.make_ortho_array(
    obj=bolt,
    v_x=App.Vector(30, 0, 0),
    v_y=App.Vector(0, 30, 0),
    v_z=App.Vector(0, 0, 0),
    n_x=3, n_y=3, n_z=1,
    use_link=True
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Interval is centre-to-centre, not gap"
    For a 20 mm part with 5 mm clearance, set Interval = 25 mm, not 5 mm.

---

## See also

- [Array Polar](array-polar.md) — ring of copies
- [Array Path](array-path.md) — copies along a curve
- [Draft Workbench](../index.md) — workbench overview
