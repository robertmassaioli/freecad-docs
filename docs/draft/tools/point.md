# Point

> **In one sentence:** Places a single point vertex object at a picked or
> typed position on the working plane.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Point  
**Command:** `Draft_Point`

Draft points are used as construction references, positions for
[Point Arrays](array-point.md), or measurement markers.

---

## Python API

```python
import Draft

pt = Draft.make_point(
    X=10, Y=20, Z=0,
    color=(1, 0, 0),
    name="MyPoint",
    point_size=5
)
App.ActiveDocument.recompute()
```

---

## See also

- [Point Array](array-point.md) — place copies of an object at point positions
- [Draft Workbench](../index.md) — workbench overview
