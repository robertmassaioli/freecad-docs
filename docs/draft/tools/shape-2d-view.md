# Shape 2D View

> **In one sentence:** Projects a 3-D solid onto the working plane as a 2-D
> Draft wire compound — a parametric orthographic projection.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Shape 2D View  
**Command:** `Draft_Shape2DView`

The result is a `Draft::Shape2DView` object linked to the source solid. If
the solid changes shape, the 2-D view updates when the document recomputes.

---

## Use cases

- Creating a flat plan view of a 3-D part for Draft annotation.
- Extracting a cross-section outline at a cut plane.
- Producing a 2-D outline for export to SVG or DXF.

---

## Projection modes

| Mode | Description |
|------|-------------|
| Solid | Visible and hidden outlines of the solid |
| Individual faces | Project one face only |
| Cutlines | Lines at the cut plane intersection |
| Cut faces | Filled faces at the cut plane |

---

## Python API

```python
import Draft

solid = App.ActiveDocument.getObject("Body")
view2d = Draft.make_shape2dview(
    solid,
    projectionVector=App.Vector(0, 0, 1)
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Does not update in real time"
    Shape 2D View updates when you trigger a document recompute
    (Edit → Refresh), not automatically on geometry changes.

---

## See also

- [Facebinder](facebinder.md) — parametric face extraction
- [Downgrade](downgrade.md) — explode a solid to faces
- [Draft Workbench](../index.md) — workbench overview
