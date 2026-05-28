# Attachment

> **In one sentence:** Attachment is the system (`Part::AttachExtension`)
> that positions datum objects and primitives relative to existing geometry —
> faces, edges, or vertices of the Body — so they update automatically when
> the solid changes.

---

## What is Attachment?

Every Part Design datum object ([Datum Plane](../tools/datum-plane.md),
[Datum Line](../tools/datum-line.md), [Datum Point](../tools/datum-point.md),
[Coordinate System](../tools/coordinate-system.md)) and every parametric
primitive (Additive Box, Additive Cylinder, etc.) inherits
**`Part::AttachExtension`**. This gives it an **Attachment** section in the
task panel and Properties panel.

Attachment lets you say: _"Place this datum plane coincident with Face3 of
the solid, and keep it there even as the solid's shape changes."_

Without attachment, datum objects would have to be repositioned manually
every time the model changes. With attachment, they follow the geometry
automatically.

---

## The Attachment dialog

Open the Attachment dialog by:
- Creating a datum object (the dialog opens automatically).
- Double-clicking an existing datum in the model tree.
- Selecting a datum and choosing **Part → Attachment…** from the menu.

The dialog has two main areas:

### Reference selector

Up to four **references** can be selected. Each reference is a
sub-element of an existing object — a face, edge, or vertex. Clicking in
the 3-D view adds references to the list.

The number and type of references required depends on the attachment mode:
- Some modes need just one face.
- Some need two edges.
- Some need three or four vertices.

FreeCAD highlights valid modes based on the references you have selected.

### Mode list

The mode list shows all attachment modes compatible with the current
references. Modes that do not match the selection are greyed out. Select
a mode from the list to preview the result in the 3-D view.

### Attachment Offset

An additional `Placement` applied **after** the attachment mode is
resolved. Use this to translate or rotate the datum away from the exact
attachment position — e.g. a plane attached to a face but offset 10 mm
outward, or rotated 45° around its normal.

---

## Attachment modes — surfaces and CSys

These modes position a coordinate system (datum plane, CSys) relative to
faces, edges, and vertices.

| Mode | UI name | References needed | What it does |
|------|---------|-------------------|-------------|
| Deactivated | Deactivated | None | Attachment disabled — position is set by Placement only |
| mmTranslate | Translate origin | 1 vertex | Origin snaps to vertex; orientation from Placement |
| mmObjectXY | Object's X Y Z | 1 object | Copies the full Placement of the referenced object |
| mmObjectXZ | Object's X Z Y | 1 object | X→X, Y→Z, Z→−Y of reference object |
| mmObjectYZ | Object's Y Z X | 1 object | X→Y, Y→Z, Z→X of reference object |
| mmFlatFace | XY on plane | 1 planar face | XY plane aligns to the selected face; Z is the face normal |
| mmTangentPlane | XY tangent to surface | 1 curved face + 1 vertex | XY plane is tangent to the surface at the vertex |
| mmNormalToPath | Z tangent to edge | 1 edge + optional vertex | Z aligns with the edge tangent at the selected point |
| mmFrenetNB | Frenet NBT | 1 edge | X→normal, Y→binormal, Z→tangent of the Frenet frame |
| mmFrenetTN | Frenet TNB | 1 edge | X→tangent, Y→normal, Z→binormal |
| mmFrenetTB | Frenet TBN | 1 edge | X→tangent, Y→binormal, Z→normal |
| mmConcentric | Concentric | 1 circular edge | Origin at centre of circle; Z along circle axis |
| mmRevolutionSection | Revolution Section | 1 edge | XZ plane passes through the edge and the revolution axis |
| mmThreePointsPlane | XY-plane by 3 points | 3 vertices | XY plane defined by three selected vertices |
| mmThreePointsNormal | XZ-plane by 3 points | 3 vertices | XZ plane defined by three vertices |
| mmFolding | Folding | 4 edges | Used for sheet metal fold operations |
| mmInertialCS | Inertial CS | 1 solid | Aligns to the principal inertia axes of the solid |
| mmParallelPlane | XY parallel to plane | 1 face + 1 vertex | XY parallel to the face, origin at the vertex |
| mmOZX | Align O-Z-X | 1 vertex + 2 edges/faces | Origin at vertex, Z towards first edge, X towards second |
| mmOZY | Align O-Z-Y | 1 vertex + 2 edges/faces | Origin at vertex, Z towards first, Y towards second |
| mmOXY | Align O-X-Y | 1 vertex + 2 edges/faces | Origin at vertex, X towards first, Y towards second |
| mmOXZ | Align O-X-Z | 1 vertex + 2 edges/faces | Origin at vertex, X towards first, Z towards second |
| mmOYZ | Align O-Y-Z | 1 vertex + 2 edges/faces | Origin at vertex, Y towards first, Z towards second |
| mmOYX | Align O-Y-X | 1 vertex + 2 edges/faces | Origin at vertex, Y towards first, X towards second |
| mmMidpoint | Midpoint | 2 vertices or 1 edge | Origin at the midpoint |

### Most commonly used plane modes

| Mode | Typical use |
|------|------------|
| **Deactivated** | Not using attachment — position manually |
| **XY on plane** | Attach a datum plane to a flat face of the solid |
| **XY parallel to plane** | Datum plane parallel to a face but at a vertex |
| **Concentric** | Datum plane at the centre of a hole or cylinder |
| **XY-plane by 3 points** | Define an angled plane through three vertices |
| **Translate origin** | Move origin to a specific vertex |

---

## Attachment modes — lines (Datum Line)

Datum Lines have their own set of modes that produce reference axes:

| Mode | UI name | References | What it does |
|------|---------|-----------|-------------|
| mm1AxisX | Object's X | 1 object | Copies the object's local X axis |
| mm1AxisY | Object's Y | 1 object | Copies the object's local Y axis |
| mm1AxisZ | Object's Z | 1 object | Copies the object's local Z axis |
| mm1AxisCurv | Edge axis | 1 circular edge | Axis of the circle or arc |
| mm1Directrix1 / 2 | 1st/2nd directrix | 1 conic | Directrix of a parabola or hyperbola |
| mm1Tangent | Tangent to edge | 1 edge + vertex | Tangent direction at the vertex |
| mm1Normal | Normal to edge | 1 edge + vertex | Normal to the edge at the vertex |
| mm1Binormal | Binormal | 1 edge + vertex | Binormal to the edge at the vertex |
| mm1TangentU / TangentV | Tangent U/V | 1 face + vertex | Tangent to face in U or V parameter direction |
| mm1TwoPoints | Line by 2 points | 2 vertices | Line connecting two vertices |
| mm1Intersection | Plane intersection | 2 planes | Line of intersection of two planes |
| mm1Proximity | Nearest distance | 2 objects | Shortest connection between two shapes |
| mm1FaceNormal | Normal to face | 1 face | Normal to the face at its centroid |

---

## Attachment modes — points (Datum Point)

Datum Points use a subset of modes for 0-D positioning:

| Mode | UI name | References | What it does |
|------|---------|-----------|-------------|
| mm0Origin | Object's origin | 1 object | Origin of the referenced object |
| mm0Focus1 / Focus2 | Focus 1/2 | 1 conic | Focal points of an ellipse or parabola |
| mm0OnEdge | On edge | 1 edge + parameter | Point at a parameter position (0–1) along the edge |
| mm0CenterOfCurvature | Centre of curvature | 1 edge + vertex | Centre of the osculating circle at the vertex |
| mm0CenterOfMass | Centre of mass | 1 solid/face | Centroid of the shape |
| mm0Intersection | Edge–edge intersection | 2 edges | Point where two edges intersect |
| mm0Vertex | Vertex | 1 vertex | Snaps to the exact vertex position |
| mm0ProximityPoint1 / 2 | Proximity point 1/2 | 2 shapes | End points of the shortest distance between two shapes |

---

## Attachment Offset

The **Attachment Offset** is a `Placement` applied after the mode is
resolved. It has:

- **Position** (x, y, z) — translate the datum away from the attachment point.
- **Rotation** — rotate around the attachment point's local axes.

### Common uses

| Use | How |
|-----|-----|
| Datum plane 10 mm above a face | Mode: XY on face; Offset Z = 10 mm |
| Datum plane rotated 45° on a face | Mode: XY on face; Offset rotation Z = 45° |
| Datum line along an edge offset inward | Mode: Tangent to edge; Offset Y = −3 mm |

---

## Attachment and parametric updates

When the referenced geometry changes (e.g. you change the diameter of a
cylinder so its flat top face moves up), the attached datum updates
automatically. This propagation continues: any sketch mapped to the datum
plane also updates, and any Pad or Pocket driven by that sketch re-evaluates.

This chain of automatic updates is the foundation of parametric Part Design.
It only works when:
1. References are set to actual geometry (face/edge/vertex of the solid), not
   free points in space.
2. The referenced topology remains stable (see the topological naming problem
   below).

---

## The topological naming problem

FreeCAD ≤ 0.20 suffered from the **topological naming problem (TNP)**:
when a feature is modified, the internal names of edges and faces
(Edge1, Face3, etc.) may be reassigned by the OCCT kernel. Any datum or
sketch referencing the old name would then reference the wrong geometry —
or fail entirely.

**FreeCAD 1.0+ includes the TNP fix (Algorithm 3).** When working in
FreeCAD 1.0 or later with a new document, topology references are stable.
If you open an old document created in FreeCAD 0.20 or earlier, re-attaching
datums and sketches may be necessary to take advantage of the fix.

---

## Python API

```python
import Part

# Get a datum plane's attachment info
dp = App.ActiveDocument.getObject("DatumPlane")
print(dp.MapMode)          # e.g. "FlatFace"
print(dp.Support)          # e.g. [(Body, ["Face3"])]
print(dp.AttachmentOffset) # Placement

# Set attachment programmatically
dp.Support = [(App.ActiveDocument.getObject("Body"), ["Face5"])]
dp.MapMode = "FlatFace"
dp.AttachmentOffset = App.Placement(
    App.Vector(0, 0, 5),   # 5 mm offset in Z
    App.Rotation()
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Attachment Offset is in the datum's local frame, not global"
    After the attachment mode positions the datum, Attachment Offset is
    applied in the datum's own local coordinate system — not the global
    XYZ. A Z offset of 10 mm moves 10 mm along the datum's local Z, which
    is the face normal for **XY on plane** mode.

!!! warning "Greyed-out modes mean the wrong reference type was selected"
    If you expect a mode to appear but it is greyed out, the selected
    reference does not match what the mode requires. A mode needing a
    planar face will be greyed if you selected a curved face or a vertex.

!!! warning "Changing the support reference resets Attachment Offset"
    If you change the referenced face (support), the Attachment Offset is
    NOT automatically scaled or rotated to compensate. Verify and adjust
    the offset after any reference change.

!!! warning "Datum planes don't update in old (pre-TNP-fix) documents"
    In documents created in FreeCAD < 1.0, topology names may shift when
    features are modified. If a datum plane suddenly points the wrong way
    after an edit, the face reference was renamed by OCCT. Re-select the
    reference face and re-set the attachment mode.

---

## See also

- [Body, Origin, and the Feature Tree](body-and-origin.md) — the Body container and the Origin's built-in planes
- [Datum Plane](../tools/datum-plane.md) — creating custom reference planes
- [Datum Line](../tools/datum-line.md) — creating custom reference axes
- [Datum Point](../tools/datum-point.md) — creating reference points
- [Coordinate System](../tools/coordinate-system.md) — full local coordinate system datum
