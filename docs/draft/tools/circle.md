# Circle

> **In one sentence:** Draws a full circle from a centre point and radius,
> optionally filled as a disc face.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Circle  
**Command:** `Draft_Circle`

---

## Key properties

| Property | Description |
|----------|-------------|
| Radius | Circle radius |
| Make Face | Fill the circle with a face (disc) |
| First Angle | If set, creates an arc instead of a full circle |
| Last Angle | End of arc if First Angle is set |

---

## Python API

```python
import Draft

circle = Draft.make_circle(radius=25, face=True)
circle.Placement.Base = App.Vector(0, 0, 0)
App.ActiveDocument.recompute()
```

---

## See also

- [Arc](arc.md) — arc (partial circle)
- [Arc by 3 Points](arc-3points.md) — arc through three points
- [Ellipse](ellipse.md) — non-circular oval
- [Draft Workbench](../index.md) — workbench overview
