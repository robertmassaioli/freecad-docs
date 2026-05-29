# Polygon

> **In one sentence:** Draws a regular polygon (equal sides and angles) by
> specifying the number of sides and a circumscribed or inscribed radius.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Polygon  
**Command:** `Draft_Polygon`

---

## Parameters

| Property | Description |
|----------|-------------|
| FacesNumber | Number of sides (3 = triangle, 6 = hexagon, etc.) |
| Radius | Circumradius (centre to vertex) |
| Draw Mode | Inscribed (vertex on radius) vs Circumscribed (edge tangent to radius) |
| Make Face | Fill with a face |

---

## Python API

```python
import Draft

hex = Draft.make_polygon(
    nfaces=6,
    radius=30,
    inscribed=True,
    face=True
)
App.ActiveDocument.recompute()
```

---

## See also

- [Rectangle](rectangle.md) — four-sided non-regular shape
- [Circle](circle.md) — infinite-sided limit case
- [Draft Workbench](../index.md) — workbench overview
