# Map Sketch to Face

> **In one sentence:** Reattach an existing sketch to a different planar face,
> axis, or datum plane without losing the sketch's geometry or constraints.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Map Sketch to Face  
**Toolbar:** Sketcher · **Shortcut:** none

Map Sketch to Face changes *where* a sketch is attached, not *what* it contains.
You select the sketch, then pick a new attachment target; FreeCAD repositions the
sketch to lie on the new face while preserving all geometry and constraints
exactly as they were.

---

## Intuition

Imagine you sketched a rectangle on a sticky note and placed it on the front face
of a box. Map Sketch to Face lets you peel the sticky note off and re-apply it to
a different face — the rectangle is unchanged, but it now lives on a new surface.

This is essential when you realise you drew a sketch on the wrong plane, or when
the model topology changes (e.g. a Pad adds a new face you now want to build on)
and you want to redirect the sketch without redrawing everything.

---

## When to use it

- You drew a sketch on the XY plane but realise it should be on a solid face that
  was created later in the feature tree.
- A topology change (renamed face, added fillet) broke the attachment reference and
  you need to re-select the correct face.
- You want to reuse a sketch profile on a different face without copying it.

## When NOT to use it

- **You want to move a sketch to a different standard plane but keep it orthogonal**
  — use [Reorient Sketch](#reorient-sketch) instead, which also lets you set a
  perpendicular offset.
- **You want a copy of the sketch on a new face** — use
  [Carbon Copy](external-geometry.md#carbon-copy) to duplicate rather than move.

---

## Step-by-step walkthrough

1. **Select the sketch** in the Model tree (click once — do not double-click, which
   would enter edit mode).

2. **Activate Map Sketch to Face** via **Sketch → Map Sketch to Face**.

3. If the document contains multiple sketches, a dialog lists them; select the one
   you want to remap and click **OK**.

4. **Select a new attachment target:**

    - Click a planar face in the 3-D viewport, or
    - Click a datum plane in the Model tree.

5. An **Attachment dialog** appears (the same dialog used when you first created the
   sketch). Choose the attachment mode — `FlatFace` is the default and correct for
   almost all use cases.

6. Click **OK**. The sketch moves to the new face. Check the model tree and
   viewport to confirm the geometry is visible on the new face.

!!! tip
    If the sketch geometry disappears after remapping, the attachment mode may be
    wrong. Re-open Map Sketch to Face and try `FlatFace` or `Translate origin`.

---

## Reorient Sketch

**Menu:** Sketch → Reorient Sketch · **Shortcut:** none

Reorient Sketch is a simpler alternative: it restricts you to the three standard
planes (XY, XZ, YZ) but also lets you set a perpendicular offset distance and flip
the sketch's local axes. Use it when the sketch belongs to a standard plane but
needs to be shifted up/down the axis.

### Walkthrough

1. Select the sketch in the Model tree.
2. **Sketch → Reorient Sketch**.
3. A confirmation dialog warns that the current attachment will be replaced. Click
   **Yes** to proceed.
4. Choose the target plane from the dropdown (XY, XZ, or YZ).
5. Optionally set **Side** (front or back of the plane) and an **Offset** value.
6. Click **OK**.

---

## Parameters

Map Sketch to Face opens the standard **Attachment** dialog. The most important
settings:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Attachment mode | Dropdown | `FlatFace` | How the sketch aligns to the selected reference. `FlatFace` places the sketch flush with a planar face. Other modes are for edge- or vertex-based attachment. |
| Reference(s) | Selection | — | The face, edge, or datum plane to attach to. Up to four references can be selected for complex attachment modes. |
| Attachment Offset | Placement | (0,0,0) | Translation and rotation applied *after* attachment. Use to nudge the sketch without changing the attachment reference. |

---

## Advanced usage

### Attachment modes beyond FlatFace

The Attachment dialog exposes over a dozen modes. For sketches, the useful ones are:

- **FlatFace** — sketch lies flush with a planar face (default).
- **Translate origin** — translate the sketch origin to a vertex without rotating.
- **Normal to edge** — align the sketch normal to a selected edge; useful for
  profiles that must follow a non-axis-aligned edge.

Most users never need to leave `FlatFace`.

### Scripting attachment

To change a sketch's attachment plane in Python:

```python
import FreeCAD as App

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Reattach to the XZ plane
sketch.AttachmentSupport = (doc.XZ_Plane, [""])
sketch.MapMode = "FlatFace"
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Sketch disappears after remapping"
    **Cause:** The selected attachment target is not a flat plane, or the attachment
    mode is incompatible with the selection.  
    **Fix:** Re-run Map Sketch to Face. Ensure you select a planar face. Use the
    `FlatFace` attachment mode.

!!! warning "Part Design feature loses its profile"
    **Cause:** A Pad or Pocket that depends on the sketch may break if the sketch
    moves to a face that is geometrically incompatible (e.g. a face that does not
    intersect the Body's current solid).  
    **Fix:** After remapping, check that the dependent features still resolve. You
    may need to adjust the feature direction (e.g. flip the Pad direction) to match
    the new face orientation.

!!! warning "Cannot find the sketch in the selection dialog"
    **Cause:** The sketch is inside a Body and the view is filtered to show only
    free objects.  
    **Fix:** Expand the Body in the Model tree and select the sketch directly before
    invoking Map Sketch to Face.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Map to the XY plane
sketch.AttachmentSupport = (doc.XY_Plane, [""])
sketch.MapMode = "FlatFace"

# Or map to a face on a solid (use its label and sub-element name)
body_feature = doc.getObject("Pad")
sketch.AttachmentSupport = [(body_feature, ["Face1"])]
sketch.MapMode = "FlatFace"

doc.recompute()
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | The face(s) or plane the sketch is attached to. |
| `MapMode` | `App::PropertyEnumeration` | The attachment mode, e.g. `"FlatFace"`. |
| `MapPathParameter` | `App::PropertyFloat` | Position along a path for edge-based attachment. |
| `AttachmentOffset` | `App::PropertyPlacement` | Fine-tune offset applied after attachment. |

---

## See also

- [New Sketch / Edit Sketch](new-sketch.md) — create a sketch and enter/exit edit mode
- [Validate Sketch](validate-sketch.md) — repair geometry after remapping
- [Datum Plane](../../part-design/tools/datum-plane.md) — create a datum plane to attach sketches to at custom angles
