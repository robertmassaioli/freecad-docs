# Datum Point

> **In one sentence:** Create a parametric reference point inside a Body that
> can snap to vertices, edge positions, face centres, intersections, and more.

---

## Overview

**Workbench:** Part Design · **Menu:** none (toolbar only) <!-- TODO: verify — no explicit Part Design menu entry found in Workbench.cpp -->  
**Toolbar:** Part Design Helper Features → Create Datum (dropdown) · **Shortcut:** none

A Datum Point is a datum object of type `PartDesign::Point` that defines a
single location in 3D space inside a Part Design Body. It has no size, no
direction, and no material contribution — its purpose is to give you a precisely
placed reference location that other features can attach to, or that scripts can
query.

Its position is governed by the *attachment* system: you select references
(vertices, edges, faces, or other datums) and an *attachment mode* that
describes which specific point on that geometry the datum should occupy. The
`PartDesign::Point` class uses `AttachEnginePoint` internally, which exposes
only the point-specific attachment modes (the `mm0*` family).

---

## Intuition

Think of a Datum Point as a push-pin you press into your model at a location
that matters — the exact centre of a face, the midpoint of an edge, the
intersection of two curves. Once placed, the pin stays at that location even
as the model changes shape. You can then attach other geometry to the pin,
measure distances from it, or read its coordinates in a script.

Because the pin is parametric, if the face moves (because a dimension changed),
the pin moves with it. No manual repositioning is needed.

---

## When to use it

- You need a reference for a sketch attachment at a point that is not a
  natural vertex of the solid (for example, the midpoint of an edge or the
  centroid of a face).
- You want to define the origin of a pattern or a local coordinate system at
  a specific geometric location.
- You need to query a model point's coordinates in a Python script in a way
  that auto-updates when the model changes.
- You are building a datum chain: a Datum Line defined from two Datum Points,
  then a Datum Plane defined normal to that line at one of the points.
- You want to mark the focus of a parabolic mirror or an ellipse arc for
  downstream calculations.

---

## When NOT to use it

- **You need a reference plane** — use [Datum Plane](datum-plane.md) instead.
- **You need a reference axis** — use [Datum Line](datum-line.md) instead.
- **You need a full coordinate frame (origin + three axes)** — use
  [Local Coordinate System](coordinate-system.md) instead.
- **You only need to constrain a sketch to an existing vertex** — use the
  Sketcher's *Point on Object* or *Coincident* constraint directly; a Datum
  Point is unnecessary overhead for in-sketch geometry.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body must exist in the document.

1. **Activate the Body** by double-clicking it in the model tree (it should
   appear bold).

2. **Optionally pre-select a vertex or edge.** If you click a vertex on the
   solid before launching the command, FreeCAD fills Reference 1 automatically
   and suggests **Vertex** mode.

3. **Launch the command.** In the *Part Design Helper Features* toolbar, click
   the **Create Datum** dropdown and choose **Datum Point**. The task panel
   opens.

4. **Select references.** Click the **Reference 1** button so it turns blue,
   then click the geometry you want to attach to (a vertex, edge, face, or
   datum object). Add Reference 2 if the chosen mode requires two references
   (for example, **Intersection Point** needs two edges or two faces).

    !!! warning "No active Body"
        If no Body is active when you launch the command, FreeCAD shows an
        error. Activate a Body first.

5. **Choose an attachment mode.** The list shows only modes compatible with
   the current references. Select the one that places the point where you want
   it. FreeCAD highlights the best suggestion automatically.

6. **Fine-tune with Attachment Offset.** Under *Attachment Offset in its Local
   Coordinate System*, enter X/Y/Z translation values to nudge the point from
   the raw attachment position while keeping the parametric link.

7. **Click OK.** The Datum Point appears in the model tree inside the Body.
   It shows as a small dot (vertex marker) in the viewport.

!!! tip
    To place a point at the midpoint of an edge, select the edge as
    Reference 1 and choose **On Edge** mode. Then set `MapPathParameter` to
    `0.5` in the Properties panel after dismissing the task panel — or set it
    to any value between 0.0 (start) and 1.0 (end) to position the point at
    any fraction along the edge.

!!! tip
    The **Center of Mass** mode works on any shape — vertex, edge, face, or
    solid. Selecting a face and choosing **Center of Mass** places the point
    at the geometric centroid of that face, which is rarely a vertex and
    therefore difficult to target with a simple coincident constraint.

---

## Parameters

All controls are in the shared *Attachment* task panel.

### References

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Object / sub-element link | — | First piece of support geometry. Can be a vertex, edge, face, datum, or origin plane. |
| Reference 2 | Object / sub-element link | — | Second reference, required by two-argument modes (Intersection Point, Proximity Point). |

### Attachment mode

`PartDesign::Point` uses `AttachEnginePoint`, which supports only the
point-placement (`mm0*`) attachment modes:

| Mode | References needed | What it does |
|------|-------------------|--------------|
| **Deactivated** | — | Point is not repositioned; stays where it was last placed. |
| **Object Origin** | Any object with a placement | Places the point at the origin of the referenced object's local coordinate system. |
| **Focus 1** | 1 conic edge (ellipse / parabola / hyperbola) | Places the point at the first focus of the conic. |
| **Focus 2** | 1 ellipse or hyperbola edge | Places the point at the second focus. |
| **On Edge** | 1 edge | Places the point along the edge at the fraction specified by `MapPathParameter` (0 = start, 1 = end, 0.5 = midpoint). |
| **Center of Curvature** | 1 edge | Places the point at the centre of curvature of the edge at `MapPathParameter`. |
| **Center of Mass** | 1 or more shapes | Places the point at the geometric centroid (centre of mass) of the selected shape(s). |
| **Intersection Point** | 2 edges, 2 faces, or 1 edge + 1 face | Places the point at the intersection of the two references. |
| **Vertex** | 1 vertex, or 1 line (uses an endpoint) | Places the point coincident with the vertex. |
| **Proximity Point 1** | 2 shapes | Places the point at the closest point on Reference 1 to Reference 2. |
| **Proximity Point 2** | 2 shapes | Places the point at the closest point on Reference 2 to Reference 1. |

### Attachment Offset

Applied after the attachment mode positions the point.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| In X-direction | Length | 0 mm | Translate the point along the world X axis (offset is applied in the attacher's local frame). |
| In Y-direction | Length | 0 mm | Translate along Y. |
| In Z-direction | Length | 0 mm | Translate along Z. |
| Around X-axis | Angle | 0° | Rotation component of the offset (affects the local frame orientation, not the point position directly for a pure point). |
| Around Y-axis | Angle | 0° | As above. |
| Around Z-axis | Angle | 0° | As above. |
| Flip sides | Bool | Off | Reverses the local frame Z axis. |

### MapPathParameter

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `MapPathParameter` | Float | 0.0 | Fractional position along the edge for **On Edge** and **Center of Curvature** modes. Range 0.0 (start vertex) to 1.0 (end vertex). Set to 0.5 for midpoint. Only active when the mode requires it. |

---

## Advanced usage

### Building a datum chain

Datum Points are the building blocks for more complex datum chains:

1. Create two Datum Points at key locations (for example, the two endpoints of
   a hole axis).
2. Create a Datum Line using **Two Point Line** mode, referencing both Datum
   Points.
3. Create a Datum Plane using **Normal to Edge** mode at one Datum Point,
   referencing the Datum Line.

When the hole moves, all three datums update automatically.

### Querying a point's coordinates in Python

After recompute, `getPoint()` returns the world position of the datum:

```python
pt = doc.getObject("DatumPoint")
doc.recompute()
pos = pt.getPoint()
print(pos)  # Base.Vector (x, y, z)
```

Alternatively, read `pt.Placement.Base` directly — both return the same
world-space coordinates.

### Using a Datum Point as a sketch attachment origin

A Datum Point can serve as the `AttachmentSupport` for a sketch in
**Object Origin** mode. This pins the sketch origin to the datum, which is
useful when you want the sketch to be anchored to a computed location (such as
a face centroid) rather than a fixed vertex.

### Intersection of two surfaces

Select two intersecting faces (or one face and one edge) as Reference 1 and
Reference 2, then choose **Intersection Point** mode. FreeCAD computes the
intersection point analytically. If the two surfaces are tangent rather than
secant, the intersection may be poorly defined — FreeCAD will warn that no
valid mode is available.

---

## Common mistakes and pitfalls

!!! warning "No attachment mode compatible with the selected references"
    **Cause:** The references you chose do not satisfy any point-placement mode.
    For example, selecting a planar face alone does not uniquely define a point
    (a face has infinitely many points).  
    **Fix:** Add a second reference that breaks the ambiguity (a second face,
    an edge, or a vertex), or choose **Center of Mass** mode to use the face
    centroid.

!!! warning "Datum Point moves to (0, 0, 0) after reopening the file"
    **Cause:** This is a known historical issue (bug #0002758). The shape stored
    for a Datum Point had its placement baked in, and restoring from file could
    lose the correct placement.  
    **Fix:** The source code has a workaround in `onDocumentRestored()` that
    recreates the internal shape from the current `Placement` after load. If
    you encounter this on an older file, open the task panel and click OK to
    force a recompute.

!!! warning "On Edge mode: point jumps to the start vertex"
    **Cause:** `MapPathParameter` defaulted to 0.0.  
    **Fix:** After dismissing the task panel, open the Properties panel, find
    `MapPathParameter`, and set it to the desired fraction (e.g. 0.5 for
    midpoint).

!!! warning "Intersection Point mode produces no result"
    **Cause:** The two selected edges or faces do not actually intersect within
    their extents, or they are tangent rather than secant.  
    **Fix:** Verify that the references genuinely intersect. For curved faces,
    switch to a Datum Line approach to find the intersection axis first, then
    add a third reference to pin the point along that axis.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create a Datum Point at the origin (using ObjectOrigin mode)
pt = doc.addObject("PartDesign::Point", "DatumPoint")
body.addObject(pt)

# Attach to the Body's XY origin plane; ObjectOrigin places the point
# at that plane's own origin = (0, 0, 0)
pt.AttachmentSupport = [(doc.XY_Plane, "")]
pt.MapMode = "ObjectOrigin"
doc.recompute()

print(pt.getPoint())
# Expected: Base.Vector (0.0, 0.0, 0.0)

# --- Place a point at the midpoint of a specific edge ---
# Assume 'some_feature' is a Part::Feature with an edge "Edge1"
# pt2 = doc.addObject("PartDesign::Point", "Midpoint")
# body.addObject(pt2)
# pt2.AttachmentSupport = [(some_feature, "Edge1")]
# pt2.MapMode = "OnEdge"
# pt2.MapPathParameter = 0.5   # midpoint
# doc.recompute()
# print(pt2.getPoint())

# --- Offset the point from the attachment result ---
pt.AttachmentOffset = App.Placement(
    App.Vector(5, 0, 0),   # 5 mm along local X
    App.Rotation()
)
doc.recompute()
print(pt.getPoint())
# Expected: Base.Vector (5.0, 0.0, 0.0)
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | List of (object, sub-element) pairs used as attachment references. |
| `MapMode` | `App::PropertyEnumeration` | Active attachment mode string, e.g. `"Vertex"`, `"OnEdge"`, `"CenterOfMass"`. `"Deactivated"` means no attachment. |
| `MapReversed` | `App::PropertyBool` | Reverses the local frame orientation (has limited effect for a pure point). |
| `AttachmentOffset` | `App::PropertyPlacement` | Additional placement applied on top of the attachment result; translates the point in the attacher's local frame. |
| `MapPathParameter` | `App::PropertyFloat` | Fraction along the edge for **On Edge** and **Center of Curvature** modes. Range 0.0–1.0. |
| `Placement` | `App::PropertyPlacement` | Final world placement. `Placement.Base` = world coordinates of the point. |
| `Shape` | `Part::PropertyPartShape` | Internal vertex shape (a `TopoDS_Vertex` at the point's location). Used by the Sketcher for attachment. |

---

## See also

- [Datum Line](datum-line.md) — reference axis; can be defined from two Datum
  Points using **Two Point Line** mode
- [Datum Plane](datum-plane.md) — reference plane using the same attachment
  system; **Three Points Plane** mode uses three Datum Points
- [Local Coordinate System](coordinate-system.md) — combines origin position
  and full axis orientation in one datum object
- [Body](body.md) — the built-in origin and its vertex, edge, and plane datums
