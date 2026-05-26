# Draft

> **In one sentence:** Taper selected faces of a solid by rotating them about a
> neutral plane, producing angled walls that allow a moulded part to release
> from its mould.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Dress-Up Features → Draft  
**Toolbar:** Part Design Dress-Up Features · **Shortcut:** none

Draft takes one or more faces on the current solid and tilts each of them by a
specified angle relative to a *neutral plane* — the plane that stays fixed while
the face rotates. The result is a tapered wall rather than a vertical one. This
is essential for plastic injection moulding and die casting, where vertical walls
would lock the part inside the mould; adding a small draft angle (typically 1°–5°)
allows the part to release cleanly.

---

## Intuition

Picture a sand castle bucket: the walls are not vertical — they slope slightly
outward from bottom to top. When you turn the bucket upside down to release the
castle, the angle allows the sand to slide out. Without the slope the sand would
grip the walls and the castle would break.

The *neutral plane* is the cross-section of the solid that does not move during
drafting — it is the "hinge" about which the face rotates. Everything on one side
of the neutral plane moves inward; everything on the other side moves outward (or
vice versa, depending on pull direction and angle sign).

The *pull direction* is the axis along which the mould is pulled — the direction
of opening. By default, FreeCAD derives the pull direction automatically from the
neutral plane's normal. You can override it with a datum line or a straight edge
if the mould opens at an angle to the part's main axes.

---

## When to use it

- You are finalising a plastic or die-cast part and every vertical wall needs a
  draft angle for mould release.
- You want the walls of a boss, rib, or housing to be slightly tapered rather
  than perfectly vertical.
- You need to apply a consistent taper angle (e.g. 1.5°) across multiple faces
  that share the same pull direction and neutral plane.
- You need to reverse the draft direction on one side of a parting line.

---

## When NOT to use it

- **You want a flat angled cut along an edge** — use [Chamfer](chamfer.md)
  instead; Draft operates on full faces, not edges.
- **You need to taper a sketch profile before padding it** — taper the sketch
  geometry or use an angled attachment plane instead.
- **The part is being machined rather than moulded** — draft angles are rarely
  needed for machined parts; use [Chamfer](chamfer.md) or [Fillet](fillet.md)
  for edge treatment.
- **You are working on a Part workbench solid outside a Body** — Draft is a
  PartDesign dress-up feature and requires an active Body.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body with at least one solid feature containing
planar or curved faces that can be drafted.

1. **Select the faces to draft.** Click one or more faces in the 3-D viewport.
   Hold ++ctrl++ to add more faces. The faces must not be the neutral plane
   itself — they are the walls that will be tilted.

2. **Open Draft.** Go to **Part Design → Dress-Up Features → Draft**, or click
   the Draft button in the *Part Design Dress-Up Features* toolbar.

3. **Set the draft angle.** Enter a value in the *Draft angle* field. The default
   is `1.5°`. Valid range: `-90°` to just below `90°`. A positive angle typically
   means the face tilts outward (adding material at the base); a negative angle
   tilts inward.

4. **Set the neutral plane.** Click the *Neutral Plane* button and then click a
   flat face or a datum plane in the viewport. The neutral plane is the
   cross-section that remains stationary — it is usually the parting plane of
   the mould.

    !!! warning "Neutral plane is required"
        If no neutral plane is set, FreeCAD attempts to guess one from the
        geometry of the first selected face. If it cannot, the operation fails
        with "No neutral plane specified and none can be guessed." Always set the
        neutral plane explicitly for reliable results.

5. **Optionally set a pull direction.** Click the *Pull Direction* button and
   select a straight edge or a datum line to override the default pull direction.
   Leave blank to use the neutral plane normal (the most common case).

6. **Optionally tick *Reversed*** to flip the direction of taper (equivalent to
   negating the angle sign).

7. **Click OK.** The Draft feature is added to the model tree.

!!! tip
    To check that your draft angles are correct, use **Part Design → Analyse →
    Draft Analysis** (if available) after applying the feature. Correct draft
    angles will show uniform colour across each face. <!-- TODO: verify menu path
    for draft analysis in FreeCAD 1.x -->

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Draft angle | Angle | `1.5°` | The tilt angle applied to each selected face. Valid range: `-90°` to just under `90°`. Positive values tilt the face in the direction of the pull direction; negative values tilt it opposite. |
| Neutral Plane | Object/sub-element link | — | The plane that stays fixed during drafting. Can be a datum plane, an `App::Plane` origin plane, a flat face of an existing feature, or a linear edge (if a pull direction is also set). If omitted, FreeCAD guesses from the selected face geometry. |
| Pull Direction | Object/sub-element link | — | The axis along which the mould is opened. Can be a datum line, an `App::Line` origin axis, or a straight edge of an existing feature. If omitted, the normal of the neutral plane is used. |
| Reversed | Toggle | off | When enabled, negates the draft angle — equivalent to multiplying the angle by −1. Useful for drafting faces on the opposite side of the neutral plane without changing the angle value. |
| References (face list) | Sub-element links | — | The faces that will be drafted. At least one face must be selected. |

---

## Advanced usage

### Neutral plane versus pull direction

These two inputs are related but distinct. The neutral plane defines *where* the
face is hinged; the pull direction defines *which way* the mould moves. In the
simplest case (a vertical mould opening along the Z axis) the neutral plane is
horizontal (the XY plane) and the pull direction is Z — and FreeCAD can derive Z
from the horizontal plane automatically.

When the mould opens at an angle to the part's main axes (a side-action or
angled-pull mould), you must set both the neutral plane and the pull direction
explicitly. The pull direction must be perpendicular to the neutral plane edge
you specify as the hinge reference.

### Drafting convex versus concave faces

A positive angle on a convex face (an external wall) adds material toward the
base. The same positive angle on a concave face (an internal cavity wall) removes
material. If you need external walls to draft outward and internal walls to draft
inward for mould release, you may need to apply two separate Draft features with
opposite signs or use *Reversed*.

### Multiple Draft features

If different groups of faces need different draft angles or different neutral
planes, apply multiple Draft features in sequence, each targeting a different set
of faces.

---

## Common mistakes and pitfalls

!!! warning "No neutral plane specified and none can be guessed"
    **Cause:** The *Neutral Plane* field is empty and FreeCAD could not
    automatically determine a hinge plane from the selected face's geometry.  
    **Fix:** Click the *Neutral Plane* button and explicitly select a datum plane
    or a flat face of the solid that should remain fixed during drafting.

!!! warning "Resulting shape is null"
    **Cause:** The draft angle or geometry produced an invalid solid — often
    because the angle is too large and the drafted face intersects another face.  
    **Fix:** Reduce the angle, or draft fewer faces at once. Also check that the
    neutral plane is on the correct side of the selected faces.

!!! warning "Pull direction reference edge must be linear"
    **Cause:** A curved edge was selected as the pull direction reference.  
    **Fix:** Select a straight edge or a datum line as the pull direction
    reference.

!!! warning "Draft looks correct but the angle is in the wrong direction"
    **Cause:** The pull direction is opposite to what you expected, or the
    selected faces are on the wrong side of the neutral plane.  
    **Fix:** Tick *Reversed* to flip the draft, or move the neutral plane to the
    other side of the faces.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign
import Sketcher
import Part

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
body.addObject(sketch)
sketch.addGeometry(Part.LineSegment(App.Vector(0,0,0), App.Vector(30,0,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(30,0,0), App.Vector(30,30,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(30,30,0), App.Vector(0,30,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(0,30,0), App.Vector(0,0,0)), False)
sketch.addConstraint(Sketcher.Constraint("Coincident", 0,2, 1,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 1,2, 2,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 2,2, 3,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 3,2, 0,1))
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = sketch
pad.Length = 20.0
doc.recompute()

# Apply a 2° draft to the four side faces, using the XY origin plane as neutral plane
draft = doc.addObject("PartDesign::Draft", "Draft")
body.addObject(draft)
# Reference the four side faces of the pad  # TODO: verify face sub-element name syntax
draft.Base = (pad, ["Face1", "Face2", "Face3", "Face4"])
draft.Angle = 2.0                            # 2 degrees
draft.NeutralPlane = (doc.XY_Plane, [""])    # XY plane as neutral plane
draft.PullDirection = None                   # derived from neutral plane normal (Z axis)
draft.Reversed = False
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Angle` | `App::PropertyAngle` | The draft taper angle in degrees. Valid range: −90° to just under 90°. Default: `1.5°`. |
| `NeutralPlane` | `App::PropertyLinkSub` | The plane that stays fixed. Accepts datum planes, `App::Plane` origin planes, flat faces, or linear edges (if `PullDirection` is also set). |
| `PullDirection` | `App::PropertyLinkSub` | The mould opening direction. Accepts datum lines, `App::Line` origin axes, or straight edges. If unset, derived from `NeutralPlane` normal. |
| `Reversed` | `App::PropertyBool` | Negates the angle direction. Default: `False`. |
| `Base` | `App::PropertyLinkSub` | Link to the base feature and the selected sub-faces. Inherited from `DressUp`. |

---

## See also

- [Fillet](fillet.md) — round edges with a smooth arc
- [Chamfer](chamfer.md) — flat angled cut along edges
- [Thickness](thickness.md) — hollow a solid to a uniform wall thickness
- [Pad](pad.md) — the most common feature to draft before finalising a moulded part
