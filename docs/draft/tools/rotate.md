# Rotate

> **In one sentence:** Rotates or copies selected objects by an angle around
> a centre point, with the rotation axis perpendicular to the working plane.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Rotate  
**Command:** `Draft_Rotate`

---

## Step-by-step

1. Select the objects.
2. Activate **Draft → Modification → Rotate**.
3. Pick the **centre of rotation**.
4. Pick a point to set the **reference direction** (angle = 0°).
5. Pick a point to set the **rotation angle** (or type a value).

---

## Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
Draft.rotate(
    [obj],
    angle=45,
    center=App.Vector(0, 0, 0),
    axis=App.Vector(0, 0, 1),
    copy=False
)
App.ActiveDocument.recompute()
```

---

## See also

- [Move](move.md) — translate objects
- [Array Polar](array-polar.md) — create a ring of rotated copies
- [Draft Workbench](../index.md) — workbench overview
