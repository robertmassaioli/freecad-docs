# Groove

> **In one sentence:** Revolve a closed 2-D profile sketch around an axis and cut
> the resulting solid out of the active Body, removing material.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Features → Groove  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Groove is the subtractive counterpart to [Revolution](revolution.md). It takes a
closed sketch, spins it around a chosen axis by a given angle, and removes the
swept volume from the Body as a Boolean cut. A full 360° sweep produces a
complete ring-shaped cavity such as an O-ring groove or a turned relief; partial
angles produce wedge-shaped pockets. Groove shares its task panel and code with
Revolution — the only difference is that the swept solid is cut away rather than
fused.

---

## Intuition

Think of a metal lathe: the cutting tool (the *profile* sketch) traces a path
around the axis of rotation (the *axis*), removing material in a swept arc. The
depth and shape of the groove are controlled entirely by the sketch; the angular
extent of the cut is controlled by the angle parameter.

The profile sketch lives on one side of the axis — the axis must not cross through
the interior of the sketch. FreeCAD rotates the sketch through space, and wherever
that swept volume overlaps the existing solid, material is removed.

A full 360° sweep gives a rotationally symmetric groove: think of the
circumferential channel on a shaft that holds a retaining ring, or the sealing
groove on a hydraulic cylinder. Partial angles give you something like an arcuate
slot or a partial turn of a thread relief.

---

## When to use it

- You are cutting a circumferential groove, channel, or relief into a shaft,
  cylinder, or any rotationally symmetric part.
- You need an O-ring or snap-ring groove around the outside or inside of a
  cylindrical feature.
- You are modelling a turned part and need to document an undercut, runout groove,
  or thread relief at the end of a threaded section.
- You want to cut a partial-angle arc slot that is wider than what a standard
  circular Pocket sketch can produce.
- You need the cut to be symmetric about the sketch plane on both sides of the
  axis (use **Symmetric to plane** mode).

---

## When NOT to use it

- **You want to add material in a rotational sweep** — use
  [Revolution](revolution.md) instead.
- **The cut travels along a straight path, not an arc** — use [Pocket](pocket.md).
- **The cut follows a curved, non-circular path** — use
  [Subtractive Pipe](subtractive-pipe.md).
- **The cut blends between different profiles** — use
  [Subtractive Loft](subtractive-loft.md).
- **You need a standard O-ring groove with controlled dimensions and tolerances**
  — consider whether a parametric sketch with the exact groove geometry in a
  [Pocket](pocket.md) might be simpler to constrain and inspect.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body that already contains an axisymmetric solid (from
a prior Revolution, Pad, or other feature). You need a closed profile sketch
positioned on one side of the intended rotation axis. The sketch must be fully
constrained.

1. **Create or select the profile sketch.** Draw the cross-section of the groove
   on a plane that passes through the rotation axis (for example, the XZ plane for
   a groove around the Z-axis). The sketch should be on one side of the axis —
   never straddle it.

2. **Activate Groove** via **Part Design → Subtractive Features → Groove**, or
   click the Groove button in the **Part Design Modeling Features** toolbar. The
   task panel opens with *Type*, *Axis*, *Angle*, and option checkboxes.

3. **Choose the Type.** `Angle` is the default and is the most common. See the
   [Parameters](#parameters) table for all options.

4. **Enter the angle** (active when Type is `Angle` or `Two angles`). The default
   is 360°. Reduce this for a partial-arc groove. Valid range: 0° < angle ≤ 360°.

5. **Choose the axis.** The *Axis* dropdown lists the sketch's own vertical and
   horizontal axes, any construction lines defined as axes inside the sketch, and
   the Body's X, Y, and Z origin axes. Pick the one that matches the rotation axis
   of the feature you are cutting. You can also click **Select reference…** and
   pick an edge from the model.

6. **Check Symmetric to plane** if the groove should extend equally in both
   angular directions about the sketch plane (available for `Angle` and
   `Through all` types).

7. **Check Reversed** if the groove is rotating in the wrong angular direction.
   FreeCAD attempts to auto-detect the correct direction; override it here if
   needed.

8. **For Up to face:** click the **Face** button and select the face of an
   existing solid feature that should bound the groove.

9. **Click OK.** The Body is updated and the Groove feature appears in the model
   tree.

!!! tip
    If you are unsure whether the groove profile is on the correct side of the
    axis, switch on the Body's origin visibility (**View → Origin**) so you can
    see the axis line while editing the sketch.

!!! warning "Axis intersects the sketch"
    If the rotation axis passes through the interior of the sketch, FreeCAD will
    report "Revolve axis intersects the sketch" and refuse to create the feature.
    Edit the sketch to ensure the entire profile is on one side of the axis.

---

## Parameters

### Type

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Type | Dropdown | `Angle` | How the angular extent of the groove is determined. See type descriptions below. |

**Type options:**

| Type | UI label | Behaviour |
|------|----------|-----------|
| `Angle` | Angle | Revolve by the angle entered in the *Angle* field. Supports *Symmetric to plane* and *Reversed*. |
| `ThroughAll` | Through all | Revolve a full 360°, cutting through the entire solid. Equivalent to entering 360° manually, but more explicit in intent. Supports *Symmetric to plane*. |
| `UpToFirst` | To first | Revolve until the swept volume first intersects an existing face. **Not yet implemented** — selecting this type produces an error at recompute time. |
| `UpToFace` | Up to face | Revolve until the swept volume reaches a selected face. The *Face* button and text field become active to select the target. Supports *Reversed*. |
| `TwoAngles` | Two angles | Revolve by *Angle* in one direction and *2nd angle* in the other direction from the sketch plane. Supports *Reversed*. |

!!! warning "To first is not yet implemented"
    Selecting **To first** (Type = `UpToFirst`) will cause the Groove feature to
    fail with the message "Groove up to first is not yet supported" when the
    document recomputes. Use **Up to face** as an alternative and select the
    relevant stopping face manually.

### Angle controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Angle | Angle | `360°` | Primary sweep angle. Active when Type is `Angle` or `Two angles`. Valid range: 0° < Angle ≤ 360°. |
| 2nd angle | Angle | `0°` | Secondary sweep angle in the opposite direction. Active only when Type is `Two angles`. |

### Axis

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Axis | Dropdown | sketch vertical axis | The rotation axis. Options include the sketch's vertical and horizontal construction axes, named construction lines from the sketch, the Body's X/Y/Z origin axes, and **Select reference…** to pick an edge from the model. |

### Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Symmetric to plane | Checkbox | Off | When on, the total angle is split equally in both directions from the sketch plane. Active for `Angle` and `Through all` types. Mutually exclusive with *Reversed*. |
| Reversed | Checkbox | Off | Reverses the direction of revolution. Disabled when *Symmetric to plane* is on. FreeCAD attempts to auto-detect the correct direction; use this to override it. |
| Face (Up to face selector) | Button + text field | — | Active only when Type is `Up to face`. Click the button to enter face-selection mode, then click the target face in the viewport. |
| Recompute on change | Checkbox | On | When on, the model updates live as you adjust parameters. Uncheck for complex models to defer recomputing until OK. |

---

## Advanced usage

### Choosing the right axis

Groove's behaviour depends entirely on choosing an axis that is parallel to the
intended rotation axis of the part. A common mistake is to pick a sketch axis that
is not aligned with the lathe or revolve axis.

If you have a construction line in the profile sketch that coincides with the
rotation axis, you can define it as a sketch axis (give it the `"Axis"` role in
the Sketcher) and it will appear in the dropdown directly.

### Two-angle grooves

`Two angles` cuts the groove across the sketch plane — Angle degrees in one
direction and 2nd angle degrees in the opposite direction. The total sweep is
Angle + 2nd angle. This is useful for cuts like radial slots that straddle a
reference plane, or for unsymmetric groove extents where the sketch is positioned
at one end.

### Symmetric to plane

When **Symmetric to plane** is checked, the total Angle is divided by two and the
groove is cut equally on both sides of the sketch plane. This is useful for a
centred circumferential groove where the profile sketch sits at the middle of the
groove's axial extent.

### Up to face

`Up to face` stops the groove exactly at a selected face. This is most useful when
the stopping boundary is not at a fixed angle but is determined by another feature
— for example, stopping a groove exactly where a bore ends.

---

## Common mistakes and pitfalls

!!! warning "Revolve axis intersects the sketch"
    **Cause:** The rotation axis passes through the interior of the profile sketch.
    OCCT cannot form a valid solid when the axis crosses the profile.  
    **Fix:** Edit the profile sketch to move the entire profile to one side of the
    axis. A minimum clearance of a fraction of a millimetre is sufficient.

!!! warning "Groove goes in the wrong direction"
    **Cause:** FreeCAD's heuristic for choosing the rotation direction chose the
    side that sweeps away from the existing solid rather than into it.  
    **Fix:** Check the **Reversed** checkbox in the task panel.

!!! warning "Angle is zero — groove has no depth"
    **Cause:** The Angle field was set to 0°, which is below the minimum valid
    angular extent.  
    **Fix:** Set Angle to any value greater than 0°. For a full groove, use 360°
    or switch to **Through all**.

!!! warning "To first produces an error on recompute"
    **Cause:** The `UpToFirst` type is present in the UI and the file format but
    is not yet implemented in the underlying engine.  
    **Fix:** Use **Up to face** instead and select the stopping face manually.

!!! warning "Result has multiple solids"
    **Cause:** The groove intersected the Body in a way that split the solid into
    separate pieces.  
    **Fix:** Verify the profile sketch does not cut entirely through the wall of
    the solid. If multiple solids are intentional, enable "Allow Compound" in
    the active Body's settings.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher
import PartDesign  # noqa: F401 — registers PartDesign types

doc = App.newDocument()

# Create a body with a base revolution (cylinder, radius 15, height 30)
body = doc.addObject("PartDesign::Body", "Body")

base_sk = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sk)
# Rectangle: X from 5 to 15, Z from 0 to 30 — revolves around Z to give a tube
base_sk.addGeometry(Part.LineSegment(App.Vector(5,  0, 0), App.Vector(15,  0, 0)), False)
base_sk.addGeometry(Part.LineSegment(App.Vector(15, 0, 0), App.Vector(15, 30, 0)), False)
base_sk.addGeometry(Part.LineSegment(App.Vector(15,30, 0), App.Vector( 5, 30, 0)), False)
base_sk.addGeometry(Part.LineSegment(App.Vector( 5,30, 0), App.Vector( 5,  0, 0)), False)
# TODO: verify — add coincident constraints to fully close the sketch
doc.recompute()

rev = doc.addObject("PartDesign::Revolution", "Revolution")
body.addObject(rev)
rev.Profile = base_sk
rev.ReferenceAxis = (doc.YZ_Plane, [""])    # Z-axis of origin
rev.Angle = 360.0
doc.recompute()

# Groove sketch: a small rectangle at mid-height for an O-ring groove
groove_sk = doc.addObject("Sketcher::SketchObject", "GrooveSketch")
body.addObject(groove_sk)
groove_sk.MapMode = "FlatFace"
groove_sk.AttachmentSupport = (doc.XZ_Plane, [""])  # XZ plane passes through Z axis
doc.recompute()

# Profile: X from 13 to 15, Z from 13 to 17 — a 2 mm deep × 4 mm wide groove
groove_sk.addGeometry(Part.LineSegment(App.Vector(13, 0, 13), App.Vector(15, 0, 13)), False)
groove_sk.addGeometry(Part.LineSegment(App.Vector(15, 0, 13), App.Vector(15, 0, 17)), False)
groove_sk.addGeometry(Part.LineSegment(App.Vector(15, 0, 17), App.Vector(13, 0, 17)), False)
groove_sk.addGeometry(Part.LineSegment(App.Vector(13, 0, 17), App.Vector(13, 0, 13)), False)
doc.recompute()

# Create the groove: full 360° cut
groove = doc.addObject("PartDesign::Groove", "Groove")
body.addObject(groove)
groove.Profile = groove_sk
groove.ReferenceAxis = (groove_sk, ["V_Axis"])  # vertical axis of sketch = Z axis
groove.Type = "Angle"
groove.Angle = 360.0
doc.recompute()

print(f"Volume after groove: {groove.Shape.Volume:.2f} mm³")  # TODO: verify exact value
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The profile sketch defining the groove cross-section. Inherited from `ProfileBased`. |
| `Type` | `App::PropertyEnumeration` | `"Angle"` | Angular extent mode. Values: `"Angle"`, `"ThroughAll"`, `"UpToFirst"`, `"UpToFace"`, `"TwoAngles"`. |
| `Angle` | `App::PropertyAngle` | `360°` | Primary sweep angle. Range: 0° to 360° (constraint: `{0.0, 360.0, 1.0}`). |
| `Angle2` | `App::PropertyAngle` | `0°` | Secondary sweep angle for `TwoAngles` type. |
| `ReferenceAxis` | `App::PropertyLinkSub` | — | The reference object and sub-element (edge, axis) used as the rotation axis. Setting this updates the internal `Axis` and `Base` properties automatically. |
| `Base` | `App::PropertyVector` | `(0, 0, 0)` | Read-only. The base point of the rotation axis, computed from `ReferenceAxis`. |
| `Axis` | `App::PropertyVector` | `(0, 1, 0)` | Read-only. The direction of the rotation axis, computed from `ReferenceAxis`. |
| `Midplane` | `App::PropertyBool` | `False` | When `True`, the groove is symmetric about the sketch plane (*Symmetric to plane* checkbox). Inherited from `ProfileBased`. |
| `Reversed` | `App::PropertyBool` | `False` | When `True`, the rotation direction is reversed. Inherited from `ProfileBased`. |
| `UpToFace` | `App::PropertyLinkSub` | — | The target face when `Type` is `"UpToFace"`. |

---

## See also

- [Revolution](revolution.md) — identical parameters, adds material instead of removing it
- [Pocket](pocket.md) — subtractive straight extrusion; the linear equivalent of Groove
- [Subtractive Pipe](subtractive-pipe.md) — cuts material along an arbitrary swept path
- [Subtractive Loft](subtractive-loft.md) — cuts material by blending through profiles
- [Pad](pad.md) — additive counterpart to Pocket
