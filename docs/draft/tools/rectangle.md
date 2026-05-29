# Rectangle

> **In one sentence:** Draws an axis-aligned rectangle by picking two diagonal
> corner points on the active working plane.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Rectangle  
**Command:** `Draft_Rectangle`

Edges are always parallel to the working plane axes. Optional fillet, chamfer,
and row/column subdivision are available as properties.

---

## Key properties

| Property | Description |
|----------|-------------|
| Length | Width of the rectangle (X direction) |
| Height | Height of the rectangle (Y direction) |
| Make Face | Fill with a face |
| Fillet Radius | Round all four corners |
| Chamfer Size | Chamfer all four corners |
| Rows / Columns | Subdivide into a grid of sub-faces |

---

## Python API

```python
import Draft

rect = Draft.make_rectangle(
    length=100,
    height=60,
    placement=App.Placement(),
    face=True
)
App.ActiveDocument.recompute()
```

---

## See also

- [Polygon](polygon.md) — regular polygon (equal sides)
- [Wire](wire.md) — arbitrary closed shape
- [Draft Workbench](../index.md) — workbench overview
