# Datum Line

> **In one sentence:** Create a parametric reference axis inside a Body that
> other features and datums can attach to.

---

## Overview

**Workbench:** Part Design · **Menu:** none (toolbar only) <!-- TODO: verify — no explicit Part Design menu entry found in Workbench.cpp -->  
**Toolbar:** Part Design Helper Features → Create Datum (dropdown) · **Shortcut:** none

A Datum Line is an infinite reference axis that lives inside a Part Design Body.
Like a Datum Plane, it adds no material — its purpose is to provide a precisely
positioned axis for attachments, revolution features, polar patterns, or any
operation that asks you to specify an axis.

---

## Intuition

Imagine drawing a thin wire through your model at exactly the angle and position
you need. That wire is the Datum Line: it has a direction and a position in space,
but no thickness and no endpoints. You can rotate features around it, align other
datums to it, or use it as the spine of a polar pattern.

The short line segment you see in the viewport is just a visual handle. The
underlying geometry extends to infinity in both directions.

Positioning works through the same *attachment* system as Datum Plane: you pick
references (faces, edges, vertices, or other datums) and choose an *attachment
mode* that describes which axis of that geometry the line should align with. An
optional *attachment offset* lets you shift or tilt the result without breaking
the parametric connection.

---

## When to use it

- You need to revolve a profile around an axis that is not the Body's built-in
  X, Y, or Z axis.
- You want to define a polar pattern axis that tracks an edge or the centre of a
  curved face as the model changes.
- You need a reference axis derived from the intersection of two planes.
- You want to build a chain of relative datums (for example, an axis 45° from a
  face normal).

---

## When NOT to use it

- **You only need a plane** — use [Datum Plane](datum-plane.md) instead.
- **You only need a point** — use [Datum Point](datum-point.md) instead.
- **The axis you need is X, Y, or Z** — use the Body's origin axes directly;
  they require no extra setup.
- **You need a complete coordinate frame with all three axes** — use
  [Local Coordinate System](coordinate-system.md).

---

## Step-by-step walkthrough

**Prerequisites:** An active Body must exist in the document.

1. **Activate the Body** by double-clicking it in the model tree.

2. **Launch the command.** In the *Part Design Helper Features* toolbar, click
   the **Create Datum** dropdown and choose **Datum Line**. The task panel opens.

3. **Select references.** Click **Reference 1** so it turns blue, then click a
   face, edge, vertex, or datum. Repeat for up to three additional references as
   needed.

    !!! warning "No active Body"
        If no Body is active, the command will show an error. Activate a Body
        first.

4. **Choose an attachment mode.** The mode list shows only the modes valid for
   the current reference set. Pick the mode that gives you the axis direction
   you need.

5. **Fine-tune with Attachment Offset.** Translate (X/Y/Z) and rotate
   (around X/Y/Z) the result if needed.

6. **Flip if needed.** Check **Flip sides** to reverse the line's direction.

7. **Click OK.** The Datum Line appears in the model tree. You can now select it
   in any command that asks for an axis.

!!! tip
    Pre-select an edge before launching the command. FreeCAD pre-fills Reference 1
    and suggests a matching mode immediately.

---

## Parameters

### References

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Object / sub-element link | — | First support geometry (face, edge, vertex, datum, origin plane/axis). |
| Reference 2 | Object / sub-element link | — | Optional second reference. |
| Reference 3 | Object / sub-element link | — | Optional third reference. |
| Reference 4 | Object / sub-element link | — | Optional fourth reference. |

### Attachment mode

The list below covers the attachment modes exposed by the `AttachEngineLine`
engine used by Datum Line. Only modes compatible with the current reference set
are enabled.

| Mode | References needed | What it does |
|------|-------------------|--------------|
| **Deactivated** | — | Line is not repositioned. |
| **Object X / Y / Z** | Any object with a placement | Aligns the line to one of the reference object's own X, Y, or Z axis. |
| **Two Point Line** | 2 vertices | Line through two selected points. |
| **Normal to Edge** | 1 edge (+ optional vertex) | Line perpendicular to the edge at the given point. |
| **Tangent** | 1 curve (+ optional vertex) | Line tangent to the curve at the given point. |
| **Binormal** | 1 curve (+ optional vertex) | Line along the Frenet binormal of the curve. |
| **Axis of Curvature** | 1 circle/arc | Line through the arc's centre along its normal axis. |
| **Intersection Line** | 2 faces | Line along the intersection of two faces. |
| **Proximity Line** | 2 shapes | Line connecting the two nearest points on two shapes. |
| **Directrix 1 / 2** | 1 conic / ellipse / hyperbola | A directrix of the selected conic curve. |
| **Asymptote 1 / 2** | 1 hyperbola | An asymptote of the hyperbola. |
| **Face Normal** | 1 curved face + 1 vertex | Line normal to the face at the vertex. |
| **Axis of Inertia 1 / 2 / 3** | 1–4 any shapes | Principal inertia axes of the selection. |

### Attachment mode in depth

Datum Line's attachment engine (`AttachEngineLine`) defines a directed line
rather than a plane. The mode determines what geometry the line tracks and
how the direction vector is derived from it.

#### Deactivated

The line stays at its last-placed position with no parametric link to model
geometry. Use the Attachment Offset to position it manually.

**Use this mode when:** Placing a line at fixed world coordinates before the
reference geometry exists, or intentionally breaking the parametric link.

**Watch out for:** A Deactivated line silently stops updating. If a datum line
does not follow model changes, check this mode first.

#### Two Point Line

A line is drawn through two selected vertices. Direction: from the first vertex
to the second. Origin: the first vertex. Like stretching a rubber band between
two pins.

**Use this mode when:**
- Defining a revolution or extrusion axis between two specific model points
- Creating a diagonal reference axis that does not coincide with any face edge

**Watch out for:** The line direction is from vertex 1 to vertex 2 — the
direction reverses if you change the selection order. Use **Flip sides** in the
Attachment Offset to reverse without reselecting.

#### Axis of Curvature

The line passes through the centre of a circular or arc edge and is directed
along its rotational axis. Like the axle through the centre of a wheel.

**Use this mode when:**
- Defining a revolution axis for a **Revolution** or **Groove** feature that
  tracks a cylindrical boss, hole, or fillet edge
- You need the axis of a torus, cylinder, or cone as a parametric reference

**Watch out for:** This is one of the most commonly useful modes — for any hole
or cylindrical feature, selecting the circular top edge gives you the axis for
free. On partial arcs the computed centre is the full-circle arc centre, not the
arc's geometric midpoint.

#### Intersection Line

The line runs along the intersection of two selected planar faces. Like the
crease at a box fold — it is the line where two flat faces meet.

**Use this mode when:**
- You need the edge between two faces as a standalone parametric axis (for
  a mirror plane intersection, a fold line, or a jig reference)
- The edge is not explicitly present in the model topology (e.g. two datum
  planes that intersect but don't produce a solid edge)

**Watch out for:** Both selected faces must be planar — the mode is invalid for
curved faces. Also, if the two faces are parallel, no intersection exists and
the mode fails.

#### Tangent

The line is tangent to a curve at a specified point, directed along the curve's
local tangent vector. Like the velocity arrow on a particle moving along a path.

**Use this mode when:**
- You need the tangent direction of a sketch curve or edge at a point (useful
  for aligning a profile sketch when starting an Additive Pipe)
- A direction that smoothly follows a curve's tangent is needed

**Watch out for:** The tangent direction flips when the MapPathParameter value
crosses a cusp or the curve direction reverses. On closed curves, the tangent
at parameter 0 = parameter 1 may differ in direction.

#### Face Normal

The line is directed along the normal to a curved face at the nearest point to
a selected vertex. Like a pin sticking out of a ball at a specific spot.

**Use this mode when:**
- You need the outward normal of a sphere, cone, or other curved surface at a
  specific angular position
- Defining a machining axis that must be perpendicular to a curved surface at a
  point

**Watch out for:** The normal direction depends on which side of the face is
considered "outward" by OCCT — this follows the face topology winding order.
If the line points inward, use Flip sides.

#### Axis of Inertia 1 / 2 / 3

The principal axes of the selection's mass distribution, ordered by moment of
inertia (1 = smallest moment = axis along the longest dimension; 3 = largest
moment = axis along the flattest dimension).

**Use this mode when:**
- You need the symmetry axis of a symmetric shape without picking a specific
  edge (e.g. the long axis of an ellipsoid or an imported casting)
- Aligning a datum to the natural orientation of a complex shape

**Watch out for:** For nearly-symmetric shapes, axis 1 and 2 may swap with
small geometry changes. These are most stable for clearly elongated or flat
shapes.

### Attachment Offset

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| In X-direction | Length | 0 mm | Translate the line along local X. |
| In Y-direction | Length | 0 mm | Translate the line along local Y. |
| In Z-direction | Length | 0 mm | Translate the line along its direction (local Z). |
| Around X-axis | Angle | 0° | Rotate around local X. Range −360° to 360°. |
| Around Y-axis | Angle | 0° | Rotate around local Y. Range −360° to 360°. |
| Around Z-axis | Angle | 0° | Rotate around local Z. Range −360° to 360°. |
| Flip sides | Bool | Off | Reverses the line direction and offsets. |

### Visual size (Size group — output properties)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| ResizeMode | Enum | `Automatic` | `Automatic`: line segment scales with the model. `Manual`: Length becomes editable. |
| Length | Length | 20 mm | Visual segment length. Read-only when `Automatic`. |

---

## Advanced usage

### Deriving a revolution axis from geometry

When you use **Part Design → Subtractive Features → Groove** or **Additive
Features → Revolution**, you are asked to pick an axis. A Datum Line is the
most flexible choice: attach it to the centre of a cylindrical face or an edge
of the solid, and the revolution will track the geometry as the model changes.

### Intersection of two planes as an axis

Select two planar faces (or Datum Planes) as references and choose the
**Intersection Line** mode. The resulting datum axis automatically lies along
the crease where those two planes would meet — useful for V-groove or hinge
geometry.

### Polar pattern axis

**Transformation Features → Polar Pattern** accepts a Datum Line as its axis.
Using an attached datum instead of a fixed world axis means the pattern stays
correctly centred even if you move the feature by changing earlier parameters.

---

## Common mistakes and pitfalls

!!! warning "No compatible attachment mode for selected references"
    **Cause:** The selected references do not constrain a unique line direction —
    for example, selecting a single flat face (which can only constrain a plane,
    not a line on its own).  
    **Fix:** Add a second reference, such as a vertex or a second edge, to
    disambiguate the axis direction.

!!! warning "Datum Line turns orange after a model change"
    **Cause:** A referenced edge, face, or vertex was renamed or deleted by a
    topological change upstream (the topological naming problem).  
    **Fix:** Re-open the Datum Line's task panel and re-select the missing
    reference.

!!! warning "Revolution uses the wrong axis direction"
    **Cause:** The line's direction may be reversed relative to what you expect.
    **Fix:** Check **Flip sides** in the task panel, or add an Attachment Offset
    rotation of 180° around Y.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

# Create a Datum Line aligned to the Body's X axis (from the XY origin plane)
line = doc.addObject("PartDesign::Line", "DatumLine")
body.addObject(line)
line.AttachmentSupport = [(doc.XY_Plane, "")]
line.MapMode = "ObjectX"  # X axis of the XY plane's parent object
doc.recompute()

print(line.Placement)
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | List of (object, sub-element) pairs used as attachment references. |
| `MapMode` | `App::PropertyEnumeration` | Active attachment mode string, e.g. `"ObjectX"`, `"TwoPointLine"`. `"Deactivated"` means no attachment. |
| `MapReversed` | `App::PropertyBool` | When `True`, the line direction is reversed and X/Y axes are mirrored. |
| `AttachmentOffset` | `App::PropertyPlacement` | Additional placement applied after the attachment result. |
| `MapPathParameter` | `App::PropertyFloat` | Parameter `[0, 1]` along a curve for edge-driven modes. |
| `ResizeMode` | `App::PropertyEnumeration` | `"Automatic"` or `"Manual"` (visual size only). |
| `Length` | `App::PropertyLength` | Visual segment length in mm. Read-only when `ResizeMode` is `"Automatic"`. Default `20`. |

---

## See also

- [Datum Plane](datum-plane.md) — reference plane, same attachment system
- [Datum Point](datum-point.md) — reference point, same attachment system
- [Local Coordinate System](coordinate-system.md) — full reference frame with three axes
- [Body](body.md) — the built-in X, Y, Z origin axes
