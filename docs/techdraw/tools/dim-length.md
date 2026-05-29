# Insert Length Dimension

> **In one sentence:** Measures the straight-line distance between two selected
> points or along a selected edge, with support for tolerances.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Dimensions → Insert Length Dimension

The measurement is the true 3-D distance if geometry references are available
(preferred), or the projected 2-D distance otherwise.

---

## Step-by-step

1. Click two **vertices** in a view (hold Ctrl to select the second).
2. Choose **TechDraw → Dimensions → Insert Length Dimension**.
3. Click to place the dimension text.

---

## Tolerances

Set tolerance values in the dimension's **Data** properties:

- `OverTolerance` / `UnderTolerance` for asymmetric (e.g. +0.05 / −0.00).
- `EqualTolerance` for symmetric (e.g. ±0.05): set `EqualTolerance = true`
  and `OverTolerance = 0.05`.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
page = doc.getObject("Page")
view = doc.getObject("FrontView")

dim = doc.addObject("TechDraw::DrawViewDimension", "Length1")
dim.Type = "Distance"
dim.References2D = [(view, "Edge1")]
page.addView(dim)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Projected dimensions on angled edges are misleading"
    A standard Length Dimension on an edge that is not parallel to the
    drawing plane measures the projected (foreshortened) length. Select 3-D
    vertices or use 3-D referenced dimensions for angled edges.

---

## See also

- [Insert Horizontal Dimension](dim-horizontal.md) — horizontal component only
- [Insert Vertical Dimension](dim-vertical.md) — vertical component only
- [Insert Dimension](dim-insert.md) — smart dimension that picks type automatically
- [TechDraw Workbench](../index.md) — workbench overview
