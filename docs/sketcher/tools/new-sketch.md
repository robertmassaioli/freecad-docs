# New Sketch / Edit Sketch / Leave Sketch

> **In one sentence:** Create a new 2-D sketch on a plane or face, enter its edit
> mode to draw and constrain geometry, then leave edit mode when the sketch is
> fully constrained and ready to drive a solid feature.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → New Sketch  
**Toolbar:** Sketcher · **Shortcut:** none

These three commands control the *lifecycle* of a sketch object: you create it,
work inside it, and then step back out. They are always the first and last actions
of any sketching session.

---

## Intuition

Think of a sketch as a tracing-paper overlay placed on a flat surface. You flip it
down onto a face (New Sketch), draw on it with constraint-aware tools (edit mode),
and then flip it back up when you are done (Leave Sketch). The tracing paper
remembers everything you drew, and Part Design can pick it up to extrude or revolve
it into 3-D solid geometry.

The three commands are deliberately separate so that you can return to a sketch at
any point — even after a Pad or Pocket has been built on top of it — and revise the
2-D geometry without losing the 3-D feature.

---

## When to use New Sketch

- You are starting a new feature (Pad, Pocket, Revolution, …) and need a 2-D
  profile as its input.
- You want to add a cross-section sketch for a Pipe or Loft spine.
- You need a standalone sketch to drive a datum constraint or a ShapeBinder.

## When NOT to use New Sketch

- **You already have a sketch and want to revise it** — double-click the sketch in
  the model tree or select it and press **Edit Sketch** instead.
- **You want to attach an existing sketch to a different face** — use
  [Map Sketch to Face](map-sketch.md) instead of creating a new sketch.

---

## Step-by-step walkthrough

### Creating a new sketch

1. **Switch to the Sketcher workbench** (or remain in Part Design — Part Design
   embeds Sketcher for sketch creation automatically).

2. **Activate New Sketch** via menu **Sketch → New Sketch** or the toolbar button.

3. **Select the attachment plane.** A dialog appears asking which plane to use:

    - **XY, XZ, or YZ** — the global coordinate planes.
    - A face on an existing solid in the viewport.
    - A datum plane you previously created.

    Click a plane or face. FreeCAD enters sketch edit mode immediately.

4. **Draw geometry and apply constraints** using the tools described in this
   workbench documentation.

5. **Leave edit mode** when finished — see [Leaving a sketch](#leaving-a-sketch)
   below.

!!! tip
    When working in Part Design, you can activate New Sketch while a Body is
    active and select any planar face of the current solid. FreeCAD positions
    the sketch automatically relative to that face.

### Editing an existing sketch

1. **Double-click the sketch** in the Model tree, or select it and choose
   **Sketch → Edit Sketch** from the menu (shortcut: none).
2. The viewport switches to sketch edit mode: the XY grid appears, the 3-D
   model is partially hidden, and the Sketcher toolbar becomes active.
3. Revise geometry and constraints.
4. **Leave edit mode** as below.

### Leaving a sketch

When you have finished drawing and the sketch is fully constrained (green):

- Press the **Close** button in the Tasks panel, or
- Choose **Sketch → Leave Sketch** from the menu, or
- Click anywhere outside the sketch viewport area.

!!! warning "Leaving with remaining degrees of freedom"
    You *can* close a sketch that is not fully constrained — FreeCAD will not
    stop you. However, Part Design features built on an under-constrained sketch
    may produce unpredictable geometry when other dimensions change. Always aim
    for "Fully constrained" (DoF = 0, all geometry green) before closing.

---

## Parameters

New Sketch has no persistent parameters. The *Attachment* (which plane the sketch
is attached to) is set at creation time and can be changed later via
[Map Sketch to Face](map-sketch.md).

---

## Advanced usage

### Creating a sketch without a Part Design body

When Sketcher is the active workbench (not Part Design), New Sketch creates a
standalone `Sketcher::SketchObject` in the document without a Body container.
This is useful for sketches that drive Part workbench operations or ShapeBinders,
but **do not** use standalone sketches as Part Design feature profiles — always
have them inside a Body.

### Attachment offset

After creating a sketch, you can fine-tune its attachment position by selecting
the sketch, opening the **Data** panel, and adjusting the `Map Path Parameter`
or `Attachment Offset` properties. This moves the sketch along or perpendicular
to its attached plane without detaching it.

---

## Common mistakes and pitfalls

!!! warning "Sketch not fully constrained before closing"
    **Cause:** Some geometry was drawn but not given enough constraints.  
    **Fix:** Watch the DoF counter in the Solver panel. Apply constraints until it
    reads "Fully constrained". Use **Sketch → Select Under-Constrained Elements**
    to highlight the remaining free geometry.

!!! warning "New sketch created outside any Body"
    **Cause:** The Sketcher workbench (not Part Design) was active when New Sketch
    was invoked, and no Body was selected.  
    **Fix:** Switch to Part Design, activate a Body, then create the sketch inside
    it. Alternatively, drag the standalone sketch into the Body in the Model tree.

!!! warning "Sketch attached to the wrong face"
    **Cause:** The wrong face was clicked during the attachment dialog.  
    **Fix:** Use [Map Sketch to Face](map-sketch.md) to reattach the sketch to the
    correct face or plane without losing its geometry.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher

doc = App.newDocument()

# Create a standalone sketch on the XY plane
sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
sketch.AttachmentSupport = (doc.XY_Plane, [""])
sketch.MapMode = "FlatFace"
doc.recompute()

# Add a line from (0,0) to (10,0)
import Part
sketch.addGeometry(Part.LineSegment(App.Vector(0, 0, 0),
                                    App.Vector(10, 0, 0)), False)
doc.recompute()

print("Sketch has", sketch.GeometryCount, "geometry elements")
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | The face, plane, or axis the sketch is attached to. |
| `MapMode` | `App::PropertyEnumeration` | How the sketch aligns to its attachment (`"FlatFace"`, `"NormalToEdge"`, etc.). |
| `AttachmentOffset` | `App::PropertyPlacement` | Fine-tune translation and rotation of the sketch relative to its attachment. |
| `MapPathParameter` | `App::PropertyFloat` | Position along a path edge for edge-based attachment modes. |
| `FullyConstrained` | `App::PropertyBool` | Read-only. `True` when the sketch has zero remaining DoF. |

---

## See also

- [Map Sketch to Face](map-sketch.md) — re-attach a sketch to a different plane or face
- [Validate Sketch](validate-sketch.md) — repair missing coincident constraints
- [Pad](../../part-design/tools/pad.md) — extrude a sketch into a solid
- [Pocket](../../part-design/tools/pocket.md) — cut into a solid using a sketch
