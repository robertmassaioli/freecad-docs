# Local Coordinate System

> **In one sentence:** Create a parametric reference frame inside a Body with
> its own origin and three labelled axes, for use as an attachment anchor in
> assembly operations or as a visual orientation guide.

---

## Overview

**Workbench:** Part Design · **Menu:** none (toolbar only) <!-- TODO: verify — no explicit Part Design menu entry found in Workbench.cpp -->  
**Toolbar:** Part Design Helper Features → Create Datum (dropdown) · **Shortcut:** none

A Local Coordinate System (LCS) is a datum object of type
`PartDesign::CoordinateSystem` that defines an origin point and three orthogonal
axes (X, Y, Z) inside a Part Design Body. Like all datum objects it adds no
material to the solid. Its positioning is controlled by the same *attachment*
system used by Datum Plane and Datum Line: you pick reference geometry and an
attachment mode, and FreeCAD solves for the resulting placement.

The LCS exposes its three axes as named sub-objects (`"X"`, `"Y"`, and the
default `""` for the Z/origin plane), which lets scripts and assemblies address
individual axes by name rather than decomposing a rotation matrix by hand.

---

## Intuition

Imagine screwing a small set of orthogonal ruler sticks to your model at a
meaningful location — perhaps the centre of a hole, the corner of a flange, or
the centre of mass of a feature group. That is the Local Coordinate System.

Unlike the Body's fixed origin, an LCS travels with the geometry it is attached
to. If the hole moves because a sketch dimension changes, the LCS snaps back to
the centre of the hole automatically. Anything downstream that references the
LCS — an assembly connector, a script that reads the axis vectors — follows
along without manual adjustment.

The coordinate triad you see in the viewport is purely visual. The real output
of an LCS is its `Placement` property (which contains the origin position and
the orientation of all three axes) together with the three `getXAxis()`,
`getYAxis()`, and `getZAxis()` unit vectors that FreeCAD can read directly.

---

## When to use it

- You are using FreeCAD's assembly tools and need to define a connector (mate
  point) on a part at a specific location and orientation.
- You want to expose a meaningful local frame to Python scripts so that they
  can query axis directions without decomposing a rotation matrix.
- You need a single datum that captures both a position and a full orientation
  (all three axes at once), which a Datum Point (position only) or Datum Line
  (one axis only) cannot provide.
- You want to build a chain of nested reference frames: one LCS positioned on
  a face, another LCS offset from the first, etc.
- You need a reference plane for a sketch that is also the origin of a set of
  dimensions measured along specific X/Y directions.

---

## When NOT to use it

- **You only need a reference plane** — use [Datum Plane](datum-plane.md)
  instead; it is lighter and has an explicit visual size you can control.
- **You only need a reference axis** — use [Datum Line](datum-line.md)
  instead.
- **You only need a reference point** — use [Datum Point](datum-point.md)
  instead.
- **You need to import a reference frame from another Body** — use
  [Sub-Shape Binder](sub-shape-binder.md) to bring the LCS placement across.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body must exist in the document.

1. **Activate the Body** by double-clicking it in the model tree (it should
   appear bold).

2. **Optionally pre-select a reference.** Click a face, edge, vertex, or
   existing datum in the 3D viewport or model tree before launching the
   command. If the selection matches an attachment mode, FreeCAD pre-fills
   the reference and suggests the mode automatically.

3. **Launch the command.** In the *Part Design Helper Features* toolbar, click
   the **Create Datum** dropdown and choose **Local Coordinate System**. The
   task panel opens.

4. **Select references.** Click the **Reference 1** button so it turns blue,
   then click the piece of geometry you want to attach to. Repeat for
   Reference 2, 3, 4 as needed. The attachment mode list updates to show only
   the valid modes for the current reference combination.

    !!! warning "No active Body"
        If no Body is active when you launch the command, FreeCAD will show an
        error message. Activate a Body first.

5. **Choose an attachment mode.** Pick the mode that positions the LCS as
   needed. For most single-face use cases, **Flat Face** places the origin on
   the face and aligns Z with the face normal. For a vertex with directional
   references, modes such as **OZX** or **OXY** give full control over axis
   orientation.

6. **Fine-tune with Attachment Offset.** Under *Attachment Offset in its Local
   Coordinate System*, enter translation (X/Y/Z) and rotation values to adjust
   the LCS away from the raw attachment result while keeping the parametric
   link.

7. **Flip if needed.** Check **Flip sides** to reverse the Z-axis direction.

8. **Click OK.** The LCS appears in the model tree inside the Body as
   `Local_CS` (or the unique name FreeCAD assigns). It displays as a colour-
   coded axis triad in the viewport (red = X, green = Y, blue = Z).

!!! tip
    Select a flat face *before* launching the command. FreeCAD will fill
    Reference 1 automatically, suggest **Flat Face** mode, and orient the LCS
    so that its Z axis points along the face normal — ideal as an assembly
    connector on a mating surface.

---

## Parameters

All controls are in the shared *Attachment* task panel.

### References

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reference 1 | Object / sub-element link | — | First piece of support geometry. Can be a face, edge, vertex, sketch, datum, or the Body's origin planes. |
| Reference 2 | Object / sub-element link | — | Optional second reference, required by some modes. |
| Reference 3 | Object / sub-element link | — | Optional third reference. |
| Reference 4 | Object / sub-element link | — | Optional fourth reference. |

### Attachment mode

The mode list shows only modes compatible with the selected references. The
`CoordinateSystem` object uses `AttachEngine3D`, the same engine as Datum Plane
and Datum Line, so the full set of 3D attachment modes is available. The most
useful modes for an LCS are:

| Mode | References needed | What it does |
|------|-------------------|--------------|
| **Deactivated** | — | LCS stays at the position it was last placed; no auto-update. |
| **Object XY / XZ / YZ** | Any object with a placement | Aligns the LCS to one of the reference object's own origin planes. |
| **Flat Face** | 1 flat face | Places the LCS origin on the face's centre, Z aligned with face normal. |
| **Concentric** | 1 circular edge | Origin at circle centre, Z along circle axis. |
| **Inertial CS** | 1–4 any shapes | Aligns the LCS to the principal inertia axes of the selected geometry. |
| **OZX / OZY / OXY / OXZ / OYZ / OYX** | 1 vertex + 1–2 edges or vertices | Origin at the vertex; axes defined by remaining references. |
| **Three Points Plane** | 3 vertices | Fits the XY plane through three points; Z is normal to that plane. |
| **Normal to Edge** | 1 edge | Z perpendicular to the edge at the nearest point. |
| **Tangent Plane** | 1 curved face + 1 vertex | Z normal to the face at the vertex. |
| **Frenet NB / TN / TB** | 1 curve | Orients axes using the Frenet–Serret frame of the curve. |

For the complete list of attachment modes and their reference requirements — with
per-mode intuition, when-to-use guidance, and gotchas — see
[Datum Plane → Attachment mode in depth](datum-plane.md#attachment-mode-in-depth).
All modes described there apply identically to the LCS.

### Attachment mode in depth (LCS-specific notes)

The LCS uses the same `AttachEngine3D` as Datum Plane, so every mode works
identically — refer to Datum Plane for the full per-mode deep dive. The
following notes cover how LCS-specific behaviour differs from a plain plane.

#### What makes LCS different from Datum Plane

A Datum Plane gives you one thing: a plane (position + normal). A Local
Coordinate System gives you three things simultaneously: an origin point, an
X axis, and a Y axis (Z is derived by right-hand rule). This matters because:

- Python code can read `lcs.getXAxis()`, `lcs.getYAxis()`, `lcs.getZAxis()`
  directly — no need to decompose the placement quaternion.
- In assembly contexts the full frame is needed, not just a plane normal.
- The LCS Z axis corresponds to the Datum Plane normal — so **Flat Face** on
  an LCS places the origin at the face centre with Z out of the face and X/Y
  tangent to the face, ready to use as an assembly interface point.

#### Recommended modes for common LCS use-cases

| Use-case | Recommended mode | Notes |
|----------|-----------------|-------|
| Interface point on a flat face (assembly) | **Flat Face** | Z = face normal; X/Y in face plane |
| Interface point at a hole centre | **Concentric** on the circular edge | Z = hole axis; origin at hole centre |
| Align LCS to another object's frame | **Object XY / XZ / YZ** | Copies the object's own coordinate system |
| Origin at a corner, axes along edges | **OZX** (or variant) | Most explicit: nail origin, aim axes |
| Match the Frenet frame of a spine | **Frenet NB / TN / TB** | For sweep path visualisation |

**Watch out for:** Unlike Datum Plane, a misaligned LCS that is used as an
assembly interface will cause mating parts to be placed at the wrong orientation
*and* the wrong position — both axes matter, not just the normal. Always check
the X axis direction in the viewport after attaching.

### Attachment Offset

Applied after the attachment mode positions the LCS.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| In X-direction | Length | 0 mm | Translate along local X. |
| In Y-direction | Length | 0 mm | Translate along local Y. |
| In Z-direction | Length | 0 mm | Translate along local Z (the primary axis). |
| Around X-axis | Angle | 0° | Rotate around local X. Range −360° to 360°. |
| Around Y-axis | Angle | 0° | Rotate around local Y. Range −360° to 360°. |
| Around Z-axis | Angle | 0° | Rotate around local Z. Range −360° to 360°. |
| Flip sides | Bool | Off | Reverses Z-axis direction and mirrors offsets. |

---

## Advanced usage

### Reading individual axis vectors in Python

`CoordinateSystem` provides `getXAxis()`, `getYAxis()`, and `getZAxis()`
methods that return unit vectors derived from the current `Placement.Rotation`.
These are more convenient than decomposing the rotation quaternion yourself:

```python
lcs = doc.getObject("Local_CS")
doc.recompute()

x = lcs.getXAxis()   # returns Base.Vector
y = lcs.getYAxis()
z = lcs.getZAxis()
print(f"Z axis direction: {z}")
```

### Sub-object access (X / Y / Z planes)

The LCS exposes three sub-objects that behave as planar references:

| Sub-name | Meaning |
|----------|---------|
| `""` (empty / default) | The XY plane of the LCS (normal = Z axis) |
| `"X"` | The YZ plane of the LCS (normal = X axis) |
| `"Y"` | The XZ plane of the LCS (normal = Y axis) |

You can select these sub-planes as attachment references for sketches or other
datums, giving full control over which face of the LCS frame is used:

```python
# Attach a datum plane to the XZ plane of the LCS
dp = doc.addObject("PartDesign::Plane", "DatumPlane")
body.addObject(dp)
dp.AttachmentSupport = [(lcs, "Y")]   # "Y" sub-object = XZ plane
dp.MapMode = "FlatFace"
doc.recompute()
```

### Nesting Local Coordinate Systems

An LCS can reference another LCS as its attachment support. This lets you build
a chain of relative frames: define one LCS on a face, then define a second LCS
offset from the first by a known distance and angle. The entire chain updates
if the base face moves.

### Using an LCS as an assembly connector

Assembly workbenches (Assembly 4, the built-in Assembly workbench in FreeCAD
1.x) typically treat an LCS's `Placement` as a connection point between parts.
Position the LCS on the mating face or at a shaft centre, name it clearly
(e.g. `LCS_BoltHole_1`), and the assembly solver can align two parts by
matching their LCS placements.

---

## Common mistakes and pitfalls

!!! warning "No attachment mode compatible with the selected references"
    **Cause:** The references you chose do not satisfy any 3D attachment mode.
    For example, selecting two parallel faces does not provide enough
    information to fix the LCS orientation around the face normal.  
    **Fix:** Add a third reference (an edge or vertex) to break the rotational
    ambiguity, or choose a different combination of references.

!!! warning "LCS turns orange after model changes"
    **Cause:** One of the references (a face, edge, or vertex) was removed or
    renumbered by an upstream topology change.  
    **Fix:** Open the LCS task panel and re-select the missing reference.

!!! warning "getXAxis() / getYAxis() / getZAxis() return wrong values"
    **Cause:** The document has not been recomputed since the LCS was moved or
    its attachment was changed.  
    **Fix:** Call `doc.recompute()` before reading the axis vectors.

!!! warning "LCS placement is correct but the sub-object plane is unexpected"
    **Cause:** The sub-names `""`, `"X"`, `"Y"` refer to the LCS's *own* XY,
    YZ, and XZ planes respectively — not the world planes. If the LCS is
    rotated, these planes are rotated too.  
    **Fix:** Verify the LCS orientation in the viewport (red = X, green = Y,
    blue = Z) and select the sub-name that corresponds to the plane you need.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create the Local Coordinate System
lcs = doc.addObject("PartDesign::CoordinateSystem", "Local_CS")
body.addObject(lcs)

# Attach the LCS to the Body's XY origin plane using FlatFace mode
lcs.AttachmentSupport = [(doc.XY_Plane, "")]
lcs.MapMode = "FlatFace"
doc.recompute()

# Read the resulting placement
print(lcs.Placement)
# Placement [Pos=(0,0,0), Yaw-Pitch-Roll=(0,0,0)]

# Read axis unit vectors
x_axis = lcs.getXAxis()
y_axis = lcs.getYAxis()
z_axis = lcs.getZAxis()
print(f"Z axis (face normal): {z_axis}")
# Base.Vector (0.0, 0.0, 1.0)

# Offset the LCS 5 mm along its Z axis
lcs.AttachmentOffset = App.Placement(
    App.Vector(0, 0, 5),
    App.Rotation()
)
doc.recompute()
print(lcs.Placement.Base)
# Base.Vector (0.0, 0.0, 5.0)
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `AttachmentSupport` | `App::PropertyLinkSubList` | List of (object, sub-element) pairs used as attachment references. |
| `MapMode` | `App::PropertyEnumeration` | Active attachment mode string, e.g. `"FlatFace"`, `"OZX"`. `"Deactivated"` means no attachment. |
| `MapReversed` | `App::PropertyBool` | When `True`, the Z axis is flipped and X axis is mirrored. |
| `AttachmentOffset` | `App::PropertyPlacement` | Additional placement applied on top of the attachment result. |
| `MapPathParameter` | `App::PropertyFloat` | Parameter `[0, 1]` along a curve for path-based attachment modes. |
| `Placement` | `App::PropertyPlacement` | Final world placement of the LCS (read after recompute). Origin = position, rotation = axis orientation. |
| `Shape` | `Part::PropertyPartShape` | Internal planar shape used by the Sketcher for attachment; not meaningful for geometry output. |

---

## See also

- [Datum Plane](datum-plane.md) — reference plane only; lighter than an LCS
  when a full coordinate frame is not needed
- [Datum Line](datum-line.md) — reference axis for revolves and polar patterns
- [Datum Point](datum-point.md) — reference point for vertex-based attachments
- [Body](body.md) — the built-in XY, XZ, YZ origin planes and origin axes
