# Scale

> **In one sentence:** Scales selected objects relative to a base point,
> with independent scale factors for each axis.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Scale  
**Command:** `Draft_Scale`

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base point | Point that stays fixed during scaling |
| X / Y / Z | Individual scale factors per axis |
| Uniform | Apply one factor to all axes equally |
| Copy | Make a scaled copy rather than scaling in place |

---

## Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
Draft.scale(
    [obj],
    scale=App.Vector(2, 2, 1),   # X×2, Y×2, Z unchanged
    center=App.Vector(0, 0, 0),
    copy=True
)
App.ActiveDocument.recompute()
```

---

## See also

- [Move](move.md) — translate objects
- [Clone](clone.md) — parametric copy with independent scale
- [Draft Workbench](../index.md) — workbench overview
