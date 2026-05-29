# Insert View

> **In one sentence:** Creates a standard orthographic projection of a
> selected 3-D object onto the drawing page — the fundamental TechDraw
> operation.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Views → Insert View

Every TechDraw drawing starts with at least one view. A view is a live link
to the 3-D geometry — it re-projects automatically when the model changes.

---

## What can be projected

- A **PartDesign Body**
- A **Part Shape** (any object with a Shape property)
- An **Assembly** (projects the full assembled state)
- A **Part::Feature** (any solid, shell, or compound)

---

## Step-by-step

1. Select the 3-D object(s) in the model tree.
2. Choose **TechDraw → Views → Insert View**.
3. The view appears on the active page. The default direction looks down the
   −Y axis (a front view in FreeCAD's coordinate convention).
4. Click the view on the page to select it. Drag it to reposition.
5. In the **Data** panel, change **Direction** to set the projection direction.

### Common projection directions

| View | Direction |
|------|-----------|
| Front | `(0, -1, 0)` |
| Top | `(0, 0, 1)` |
| Right | `(1, 0, 0)` |
| Isometric | `(1, -1, 1)` (normalised) |

---

## View properties (Data panel)

| Property | Description |
|----------|-------------|
| Direction | Projection direction vector |
| Scale | Drawing scale ratio (1.0 = 1:1) |
| Scale Type | Custom / Page / Automatic |
| Rotation | Rotation of the view on the page (degrees) |
| CoarseView | Faster rendering at lower quality (useful for large assemblies) |
| ShowHidden | Show hidden edges as dashed lines |
| ShowSmoothLines | Show smooth (tangent) edge transitions |
| ShowSeamLines | Show seam lines on surfaces |

---

## Python API

```python
import FreeCAD as App
import TechDraw

doc = App.ActiveDocument
page = doc.getObject("Page")
body = doc.getObject("Body")

view = doc.addObject("TechDraw::DrawViewPart", "FrontView")
page.addView(view)
view.Source = [body]
view.Direction = App.Vector(0, -1, 0)   # Front view
view.Scale = 1.0
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Default front view looks along –Y"
    TechDraw's default front view looks along −Y. FreeCAD's PartDesign
    convention places the XY plane as the sketch plane, so the default
    front view of a PartDesign body often shows the bottom face. Adjust
    the **Direction** property or set a custom direction in the task panel.

!!! warning "Renaming or deleting the source body breaks the view"
    If you rename or delete a body that a view references, the view displays
    a red warning and stops updating. Re-select the source object in the
    view's **Source** property.

---

## See also

- [Insert Projection Group](view-projection-group.md) — create a full multi-view set
- [Insert Section View](view-section.md) — cut-through cross-section
- [Insert Detail View](view-detail.md) — magnified inset
- [TechDraw Workbench](../index.md) — workbench overview
