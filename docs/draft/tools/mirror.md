# Mirror

> **In one sentence:** Creates a mirrored copy of selected objects across a
> mirror line defined by two picked points in the working plane.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Mirror  
**Command:** `Draft_Mirror`

Mirror always creates a copy — the original is preserved.

---

## Step-by-step

1. Select the objects.
2. Activate **Draft → Modification → Mirror**.
3. Pick the **first point** of the mirror line.
4. Pick the **second point** of the mirror line.

---

## Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
mirrored = Draft.mirror(
    [obj],
    p1=App.Vector(0, 0, 0),
    p2=App.Vector(0, 100, 0)
)
App.ActiveDocument.recompute()
```

---

## See also

- [Move](move.md) — translate with optional copy
- [Clone](clone.md) — parametric linked copy
- [Draft Workbench](../index.md) — workbench overview
