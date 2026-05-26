# Datum Plane

> **In one sentence:** Create a parametric reference plane inside a Body that
> sketches and features can attach to.

---

## Overview

**Workbench:** Part Design · **Menu:** none (toolbar only) <!-- TODO: verify — no explicit Part Design menu entry found in Workbench.cpp -->  
**Toolbar:** Part Design Helper Features → Create Datum (dropdown) · **Shortcut:** none

A Datum Plane is an infinite, flat reference surface that lives inside a Part
Design Body. It is not a solid feature — it adds no material and removes none.
Its sole purpose is to give you a custom plane to sketch on, to mirror features
across, or to use as a reference for attachments, when the Body's built-in XY,
XZ, and YZ planes are not in the right position or orientation.

---

## Intuition

Think of a Datum Plane as a transparent piece of glass you can slide and tilt
anywhere inside your model. Once placed, it behaves exactly like one of the
Body's built-in origin planes: you can open a new sketch on it, attach another
datum to it, or reference it in a pattern or mirror operation.

The glass itself is infinite in extent — the small rectangle you see in the
viewport is just a visual handle. What matters is the plane's *position* and
*normal direction*, both of which are controlled by the references (support
geometry) you choose.

The positioning system is called *attachment*. You pick between one and four
pieces of reference geometry (faces, edges, vertices, or other datums), choose
an *attachment mode* that describes how the plane should relate to that geometry,
and optionally add an *attachment offset* to nudge the result without breaking
the parametric link.

---

## When to use it

- You need to sketch on a face that is not perpendicular to any of the three
  origin planes (for example, an angled boss on an inclined face).
- You want to place a mirror plane at the midpoint of your solid, offset from
  the origin.
- You need a plane that tracks a face of your model so that downstream features
  stay in the right place as the model changes.
- You are building a family of parts and need a plane whose position is driven
  by a named parameter (use **Attachment Offset** or attach to a sketch point
  that is itself driven by a formula).
- You want to create a symmetric feature without sketching directly on the solid.

---

## When NOT to use it

- **You only need to reorient an existing sketch** — use
  [Sketch → Map Sketch to Face](../../sketcher/tools/map-sketch.md) instead;
  it repositions the sketch without creating a persistent datum object.
- **The plane is one of XY, XZ, or YZ** — use the Body's built-in origin planes
  directly; they are always available and require no extra steps.
- **You need a reference axis, not a plane** — use
  [Datum Line](datum-line.md) instead.
- **You need a reference point** — use [Datum Point](datum-point.md) instead.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body must exist in the document.

1. **Activate the Body** by double-clicking it in the model tree (it should
   appear bold).

2. **Launch the command.** In the *Part Design Helper Features* toolbar, click
   the **Create Datum** dropdown and choose **Datum Plane**. The task panel
   opens immediately.

3. **Select references.** Click the **Reference 1** button in the task panel so
   it turns blue, then click a face, edge, vertex, or existing datum in the
   viewport or model tree. The reference name appears in the text field.
   Repeat for up to three additional references if needed. The mode list updates
   automatically to show only the attachment modes that are valid for the
   references you have chosen.

    !!! warning "No active Body"
        If no Body is active, the command will show an error. Activate a Body
        before inserting any datum object.

4. **Choose an attachment mode.** In the *Attachment mode* list, click the mode
   that places the plane where you want it. FreeCAD highlights the best-matching
   mode for your reference set automatically — you can leave the suggestion or
   pick a different one.

5. **Fine-tune with Attachment Offset.** Under *Attachment Offset in its Local
   Coordinate System*, enter translation (X/Y/Z) and rotation (around X/Y/Z)
   values to shift or tilt the plane away from the raw attachment result without
   losing the parametric link.

6. **Flip if needed.** Check **Flip sides** to reverse the plane's normal
   direction (and mirror the offset).

7. **Click OK.** The Datum Plane appears in the model tree inside the Body. It
   displays as a small, semi-transparent rectangle in the viewport. You can now
   click it in the tree and open a new sketch on it.

!!! tip
    Select a face on the solid *before* launching the command. FreeCAD
    pre-fills Reference 1 with that face and immediately suggests a valid mode,
    saving a click.

---

## Parameters

All controls are in the shared *Attachment* task panel.

### References

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Object / sub-element link | — | First piece of support geometry. Can be a face, edge, vertex, sketch, datum, or the Body's origin planes. |
| Reference 2 | Object / sub-element link | — | Optional second reference, required by some attachment modes. |
| Reference 3 | Object / sub-element link | — | Optional third reference. |
| Reference 4 | Object / sub-element link | — | Optional fourth reference. |

### Attachment mode

The list shows only the modes that are valid for the currently selected
references. The most useful modes for a Datum Plane are:

| Mode | References needed | What it does |
|------|-------------------|--------------|
| **Deactivated** | — | Plane is not repositioned; stays where it was last placed. |
| **Object XY / XZ / YZ** | Any object with a placement | Aligns the plane to one of the reference object's own base planes. |
| **Flat Face** | 1 flat face | Aligns the plane to coincide with the selected flat face. |
| **Tangent Plane** | 1 curved face + 1 vertex | Places the plane tangent to the face at the vertex. |
| **Normal to Edge** | 1 edge | Places the plane perpendicular to the edge. |
| **Three Points Plane** | 3 vertices | Fits the plane through three chosen points. |
| **Three Points Normal** | 3 vertices | As above but normal faces away from the points. |
| **Parallel Plane** | 1 face + 1 vertex | Plane parallel to the face, passing through the vertex. |
| **Frenet NB / TN / TB** | 1 curve | Orients the plane using the Frenet–Serret frame of the curve. |
| **Concentric** | 1 circle/arc | Plane through the circle's centre, normal to its axis. |
| **Inertial CS** | 1–4 any shapes | Aligns the plane using the principal inertia axes of the selected geometry. |
| **OZX / OZY / OXY / OXZ / OYZ / OYX** | 1 vertex + 1–2 edges/vertices | Origin-at-vertex, axes defined by the remaining references. |
| **Folding** | 4 lines | Derives a crease plane from four coplanar lines. |

### Attachment Offset

Applied after the attachment mode positions the plane.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| In X-direction | Length | 0 mm | Translate the plane along its local X axis. |
| In Y-direction | Length | 0 mm | Translate the plane along its local Y axis. |
| In Z-direction | Length | 0 mm | Translate the plane along its normal (local Z). |
| Around X-axis | Angle | 0° | Rotate around the local X axis. Range −360° to 360°. |
| Around Y-axis | Angle | 0° | Rotate around the local Y axis. Range −360° to 360°. |
| Around Z-axis | Angle | 0° | Rotate around the local Z axis. Range −360° to 360°. |
| Flip sides | Bool | Off | Reverses the plane normal and offsets. |

### Visual size (Size group — output properties, not editable by default)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| ResizeMode | Enum | `Automatic` | `Automatic`: the viewport rectangle scales with the model. `Manual`: Length and Width become editable. |
| Length | Length | 20 mm | Visual length of the rectangle (output only when `Automatic`). |
| Width | Length | 20 mm | Visual width of the rectangle (output only when `Automatic`). |

---

## Advanced usage

### Stacking datum planes

A Datum Plane can reference another Datum Plane as its support. This lets you
build chains: one plane is defined relative to a face, a second is offset from
the first, a third is angled relative to the second. When the base face moves,
all downstream planes update automatically.

### Attaching sketches to a Datum Plane

After creating a Datum Plane, right-click it in the model tree and choose
**New Sketch** — or create a new sketch and use **Sketch → Map Sketch to Face**
and click the datum plane. Either way, the sketch will be locked to the plane
and will follow it if the plane's attachment updates.

### Using a Datum Plane as a mirror plane

**Part Design → Transformation Features → Mirrored** accepts a Datum Plane as
its mirror object. This lets you mirror patterns around a plane that tracks your
geometry rather than a fixed origin plane.

---

## Common mistakes and pitfalls

!!! warning "No attachment mode compatible with the selected references"
    **Cause:** The references you selected do not satisfy any known attachment
    mode (e.g. three parallel edges — not enough constraints to define a unique
    plane).  
    **Fix:** Change or add references until at least one mode appears highlighted
    in the list. The status label at the top of the task panel indicates what the
    current selection can produce.

!!! warning "Datum Plane disappears or turns orange after model changes"
    **Cause:** One of the references (a face, edge, or vertex) was deleted or
    renamed by a topological change upstream. The attachment engine can no longer
    resolve the placement.  
    **Fix:** Open the Datum Plane's task panel and re-select the missing
    reference.

!!! warning "Sketch drifts after editing the Datum Plane's attachment"
    **Cause:** The sketch was created before the Datum Plane was repositioned.
    The sketch's attachment stores the Datum Plane object but not an updated
    transform.  
    **Fix:** Re-map the sketch: right-click it → **Attach Sketch** → click the
    Datum Plane again.

!!! warning "OK is clicked but the mode is Deactivated"
    **Cause:** FreeCAD warned that no mode fits the references, and you chose to
    continue. The plane will not update as the model changes.  
    **Fix:** Re-open the task panel and select references that enable at least
    one attachment mode.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401 — registers PartDesign types

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create a Datum Plane attached to the Body's XY origin plane
plane = doc.addObject("PartDesign::Plane", "DatumPlane")
body.addObject(plane)
plane.AttachmentSupport = [(doc.XY_Plane, "")]  # Body origin XY plane
plane.MapMode = "FlatFace"
doc.recompute()

print(plane.Placement)
# Expected: Placement [Pos=(0,0,0), Yaw-Pitch-Roll=(0,0,0)]
```

To offset the plane 10 mm in its normal direction:

```python
import FreeCAD as App

plane.AttachmentOffset = App.Placement(
    App.Vector(0, 0, 10),   # 10 mm along Z (the plane's normal)
    App.Rotation()          # no rotation
)
doc.recompute()
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | List of (object, sub-element) pairs used as attachment references. |
| `MapMode` | `App::PropertyEnumeration` | Active attachment mode string, e.g. `"FlatFace"`, `"ObjectXY"`. `"Deactivated"` means no attachment. |
| `MapReversed` | `App::PropertyBool` | When `True`, the plane normal is flipped and the X axis is mirrored. |
| `AttachmentOffset` | `App::PropertyPlacement` | Additional placement applied on top of the attachment result. |
| `MapPathParameter` | `App::PropertyFloat` | Parameter `[0, 1]` along a curve for the `NormalToEdge` mode. |
| `ResizeMode` | `App::PropertyEnumeration` | `"Automatic"` or `"Manual"` (visual size only; does not affect geometry). |
| `Length` | `App::PropertyLength` | Visual rectangle length in mm. Read-only when `ResizeMode` is `"Automatic"`. Default `20`. |
| `Width` | `App::PropertyLength` | Visual rectangle width in mm. Read-only when `ResizeMode` is `"Automatic"`. Default `20`. |

---

## See also

- [Datum Line](datum-line.md) — reference axis, same attachment system
- [Datum Point](datum-point.md) — reference point, same attachment system
- [Local Coordinate System](coordinate-system.md) — reference frame with all three axes
- [Additive Pipe](additive-pipe.md) — sweep along a path; datum planes are often used as section planes
- [Body and Origin](../concepts/body-and-origin.md) — the built-in XY, XZ, YZ planes
