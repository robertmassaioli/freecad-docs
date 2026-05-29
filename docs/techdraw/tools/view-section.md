# Insert Section View

> **In one sentence:** Creates a cross-section view by cutting an existing
> view with a plane, exposing internal geometry with hatching on the cut faces.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Views → Insert Section View

A section view reveals internal geometry (holes, bores, pockets) that would
otherwise only be visible as hidden dashed lines. The cut faces are
automatically hatched.

---

## Step-by-step

1. Select an existing view on the page (the parent view to cut).
2. Choose **TechDraw → Views → Insert Section View**.
3. In the task panel:
   - Set the **Cut Direction** (normal to the cut plane).
   - Position the cut plane by dragging the line on the parent view, or
     enter an exact offset value.
4. Click **OK**. The section view appears, labelled automatically (A–A, B–B, …).

---

## Properties

| Property | Description |
|----------|-------------|
| BaseView | The parent view that is cut |
| Normal | Normal vector of the cutting plane |
| Origin | A point on the cutting plane |
| Scale | Independent scale for the section view |
| ShowCutSurface | Show hatching on cut faces |
| CutSurfaceDisplay | Hatching pattern or colour |

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
page = doc.getObject("Page")
front_view = doc.getObject("FrontView")

section = doc.addObject("TechDraw::DrawViewSection", "SectionA")
page.addView(section)
section.BaseView = front_view
section.SectionNormal = App.Vector(0, 1, 0)    # cut plane faces Y
section.SectionOrigin = App.Vector(0, 0, 0)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Section view requires a parent view"
    Section View must be created from an existing parent view. You cannot
    apply a section cut to the 3-D model directly — create a standard view
    first, then cut it.

---

## See also

- [Insert Complex Section](view-complex-section.md) — offset or aligned (bent) section
- [Insert View](view-insert.md) — create the parent view first
- [Hatch a Face](hatch-face.md) — add hatching to cut faces manually
- [TechDraw Workbench](../index.md) — workbench overview
