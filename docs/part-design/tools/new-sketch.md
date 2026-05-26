# New Sketch

> **In one sentence:** Create a new Sketcher sketch attached to a plane or face
> and open it for editing inside the active Part Design Body.

---

## Overview

**Workbench:** Part Design · **Menu:** Sketch → New Sketch  
**Toolbar:** Part Design Helper Features (via the "Create Datum" dropdown) · **Shortcut:** none

New Sketch creates a `Sketcher::SketchObject` inside the active Body and
immediately opens it in the Sketcher editor. The sketch is attached to a chosen
plane or planar face, which defines its coordinate system. Once closed, the
sketch is available as a profile for any sketch-based feature such as Pad,
Pocket, Revolution, or Additive Pipe.

---

## Intuition

Think of a sketch as a sheet of graph paper fixed to a surface. Before you can
draw on it you must first decide which surface to stick it to — a principal
plane (XY, XZ, or YZ), a datum plane you have already created, or a flat face
on an existing solid. That choice determines which direction your 2-D drawing
will face in 3-D space.

New Sketch is the entry point for almost every Part Design workflow. Every solid
feature that requires a profile begins here. Unlike the Sketcher's own "New
Sketch" command (which creates a freestanding sketch outside any Body), this
command always creates the sketch *inside* the active Body so that subsequent
features can use it.

The command checks what you have selected when you invoke it. If a face or plane
is already selected, FreeCAD attaches the sketch there immediately and opens the
editor. If nothing is selected but the Body has exactly one valid plane, the
sketch is placed on that plane without asking. If multiple valid planes exist, a
plane-picker dialog appears so you can choose.

---

## When to use it

- You are starting a new Body and need to draw the first profile for a Pad or
  Revolution.
- You want to add another sketch mid-way through a feature tree to serve as a
  profile for a later Additive Pipe or Loft.
- You want to draw on a specific planar face of an existing solid — for example,
  the top face of a Pad — to build the next feature on top of it.
- You need a new sketch on a datum plane you have created with Part Design Plane.
- You are restarting after closing the Sketcher and want to edit a different
  cross-section without touching existing features.

---

## When NOT to use it

- **You want to edit an existing sketch** — double-click the sketch in the model
  tree, or use Sketch → Edit Sketch instead.
- **You want to reattach a sketch to a different plane** — use
  Sketch → Map Sketch to Face (`Sketcher_MapSketch`).
- **You are working outside a Body and want a freestanding 2-D sketch** — use
  the Sketcher workbench's own New Sketch command. Part Design's New Sketch
  always requires an active Body.

---

## Step-by-step walkthrough

**Prerequisites:** An open FreeCAD document. A Body will be created automatically
if one does not exist yet.

1. **Optionally pre-select a plane or face.** Click a principal plane (XY, XZ,
   or YZ in the Origin), a PartDesign Plane datum, or a flat face on an existing
   solid in the viewport before invoking the command. Pre-selecting skips the
   plane-picker dialog.

2. **Invoke New Sketch.** Go to Sketch → New Sketch in the menu bar, or click
   the New Sketch icon in the "Part Design Helper Features" toolbar (inside the
   "Create Datum" dropdown group).

3. **Select a plane if prompted.** If you had nothing pre-selected and multiple
   valid planes are available, a plane-picker dialog appears. Click the desired
   plane and press OK.

    !!! warning "No valid planes error"
        If no Body exists and one cannot be created, or if the Body has no valid
        plane (which can happen in edge cases with misplaced insert points),
        FreeCAD will show a warning. Create a Body manually first, or select a
        face before invoking the command.

4. **Draw in the Sketcher.** The Sketcher editor opens. Draw your geometry and
   apply constraints until the sketch is fully constrained (all lines turn
   green).

5. **Close the Sketcher.** Click "Close" in the task panel or press **Esc**. The
   sketch appears in the model tree under the Body, ready to be used as a profile
   for a feature.

!!! tip
    If you click a flat face on an existing solid *before* invoking New Sketch,
    the sketch is attached directly to that face with `MapMode = FlatFace`.
    This is the fastest way to build stacked features — click the top face of
    a Pad, then invoke New Sketch.

---

## Parameters

New Sketch itself has no task-panel parameters — the command creates a
`Sketcher::SketchObject` and opens it for editing. The resulting sketch object
has the following key properties visible in the property panel after closing the
editor:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | — | The plane, face, or datum the sketch is attached to. Set automatically when you choose a plane. |
| `MapMode` | `App::PropertyString` | `FlatFace` | Attachment mode. `FlatFace` when attached to a face or plane. `ObjectXY` when attached to another sketch. |
| `MapReversed` | `App::PropertyBool` | `False` | Flips the sketch normal (reverses which side of the support plane the Z axis points). |
| `Placement` | `App::PropertyPlacement` | — | Computed from the attachment; normally read-only while mapped. |

---

## Advanced usage

### Sketching on an external face

If you pre-select a face that belongs to a solid *outside* the active Body,
FreeCAD asks whether to create a cross-reference (an xref link), an
independent copy, or a dependent copy. Choose:

- **Cross-reference** to keep a live link — the sketch attachment updates when
  the external solid moves.
- **Independent copy** to capture the face geometry once without any ongoing
  link.
- **Dependent copy** to create a Shape Binder of the face and attach to that.

### Multiple sketches on the same plane

A Body can contain any number of sketches on the same plane. Each is an
independent object in the feature tree. The insertion point (the "Tip" marker)
determines which features are visible at any given time, but all sketches remain
in the tree.

### Using datum planes as sketch supports

Creating a datum plane (Part Design → Datum Plane) before invoking New Sketch
gives you complete freedom to place the sketch at any position and angle. Once
the datum plane exists, it appears in the plane-picker dialog alongside the
principal planes.

---

## Common mistakes and pitfalls

!!! warning "Sketch created outside the Body"
    **Cause:** No Body was active when New Sketch was invoked, and FreeCAD
    created a new Body automatically, but you already had features in a
    different Body.  
    **Fix:** Activate the intended Body first (double-click it in the model
    tree), then invoke New Sketch again.

!!! warning "Sketch is not fully constrained but was closed"
    **Cause:** Lines are still blue (under-constrained) or red (over-constrained)
    when you close the Sketcher.  
    **Fix:** Re-open the sketch (double-click it), add the missing constraints,
    and close again. Attempting to create a Pad or Pocket from an under-constrained
    sketch will succeed but the shape will change if you later edit the sketch.

!!! warning "Non-planar face selected as support"
    **Cause:** A curved face (cylinder, sphere) was selected before invoking
    New Sketch.  
    **Fix:** Select only flat (planar) faces, or use a datum plane positioned
    tangent to the surface.

!!! warning "MapMode resets unexpectedly after model change"
    **Cause:** The face the sketch is attached to was renumbered by a topology
    change (topological naming problem).  
    **Fix:** Use a datum plane instead of a direct face reference, or enable
    toponaming features available in FreeCAD 1.0+.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create a sketch attached to the XY plane
sketch = body.newObject("Sketcher::SketchObject", "Sketch")
sketch.AttachmentSupport = (doc.XY_Plane, [""])
sketch.MapMode = "FlatFace"
doc.recompute()

# Add a rectangle 20 × 10 mm centred on the origin
sketch.addGeometry([
    Part.LineSegment(App.Vector(-10, -5, 0), App.Vector(10, -5, 0)),
    Part.LineSegment(App.Vector(10, -5, 0),  App.Vector(10,  5, 0)),
    Part.LineSegment(App.Vector(10,  5, 0),  App.Vector(-10,  5, 0)),
    Part.LineSegment(App.Vector(-10,  5, 0), App.Vector(-10, -5, 0)),
], False)

doc.recompute()
print(f"Sketch is fully closed: {sketch.Shape.isClosed()}")
```

### Key properties on the resulting sketch object

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | Plane or face the sketch is attached to. |
| `MapMode` | `App::PropertyString` | Attachment mode string (e.g. `"FlatFace"`, `"ObjectXY"`). |
| `MapReversed` | `App::PropertyBool` | `True` to flip the sketch normal. |
| `Geometry` | `Part::PropertyGeometryList` | The geometric elements (lines, arcs, …) inside the sketch. |
| `Constraints` | `Sketcher::PropertyConstraintList` | All Sketcher constraints applied to the geometry. |

---

## See also

- [Shape Binder](shape-binder.md) — reference geometry from another body to use as a sketch support
- [Sub-Shape Binder](sub-shape-binder.md) — more flexible cross-body reference
- [Pad](pad.md) — the most common feature to create from a closed sketch
- [Pocket](pocket.md) — remove material using a closed sketch
