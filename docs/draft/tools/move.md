# Move

> **In one sentence:** Moves or copies selected Draft objects by a displacement
> vector defined by two picked points.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Move  
**Command:** `Draft_Move`

---

## Step-by-step

1. Select the objects to move.
2. Activate **Draft → Modification → Move**.
3. Pick the **base point** (origin of the displacement).
4. Pick the **target point** (destination).
5. Optionally press **P** to make a copy instead of moving.

---

## Options during operation

| Key | Action |
|-----|--------|
| P | Toggle copy mode (make a copy, leave original) |
| A | Toggle relative mode (target is offset from base) |
| G | Toggle global mode (use global coordinates) |

---

## Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
Draft.move([obj], App.Vector(50, 0, 0), copy=False)
App.ActiveDocument.recompute()
```

---

## See also

- [Rotate](rotate.md) — rotate around an axis
- [Scale](scale.md) — scale relative to a base point
- [Snapping](snap-lock.md) — precise base/target point placement
- [Draft Workbench](../index.md) — workbench overview
