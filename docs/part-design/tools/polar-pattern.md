# Polar Pattern

> **In one sentence:** Repeat selected features around a rotation axis to produce a
> circular array of evenly spaced (or explicitly positioned) copies.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Transformation Features → Polar Pattern  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Polar Pattern takes one or more existing Body features and rotates them around
a specified axis to create multiple copies at angular intervals. Each copy is
fused into the Body in the same way as the original — additive features add
material, subtractive features cut it. Because the copies are parametric
references to the originals, any change to the original feature propagates
automatically to every copy.

---

## Intuition

Think of a bolt circle: six identical tapped holes arranged at 60° intervals
around the centre of a flange. Instead of drawing and pocketing each hole
individually, you create one hole and instruct Polar Pattern to rotate it five
more times around the flange's central axis. Change the hole diameter and all
six update instantly.

The rotation axis determines the centre of the circle and its orientation in
3D space. The pattern places copies at equal angular steps across the total
**Angle** range — or at a fixed angular **Spacing** per step.

Like all transformation features in Part Design, Polar Pattern operates on
*features*, not on the whole Body. The bolt circle holes are patterned; the
flange itself is not.

---

## When to use it

- You have a repeated feature (hole, boss, slot, cut) that should appear at
  regular angular positions around an axis of symmetry.
- You are modelling a gear, fan blade, rotor, sprocket, or any part with
  rotational periodicity.
- You want full 360° symmetry: set Angle to 360° and choose the total number
  of copies.
- You need partial symmetry (e.g. three holes over 90°): reduce Angle
  accordingly.

---

## When NOT to use it

- **The copies lie along a straight line** — use [Linear Pattern](linear-pattern.md)
  instead.
- **You need exactly one reflected copy across a plane** — use
  [Mirror](mirrored.md); it has a simpler setup for that case.
- **You want to combine a polar array with a linear array or a mirror** — use
  [Multi-Transform](multi-transform.md).
- **The axis of rotation varies per copy** (e.g. a helical arrangement) — Polar
  Pattern does not support this; use separate features or scripting.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one sketch-based feature to
pattern, and a reference for the rotation axis — an origin axis (X, Y, or Z),
a datum line, a sketch axis, a straight edge, or a circular edge (whose axis is
used automatically).

1. **Select the feature(s) to pattern.** Click one or more features in the
   model tree. You can also skip this step and add them inside the task panel.

2. **Activate Polar Pattern.** Go to **Part Design → Transformation Features
   → Polar Pattern**. The task panel opens.

3. **Choose transform scope.** Select **Transform body** to pattern the whole
   Body solid, or **Transform tool shapes** to pattern only the listed features.
   For feature-level patterning, keep **Transform tool shapes**.

4. **Add features.** Use **Add Feature** / **Remove Feature** to populate the
   feature list.

5. **Set the Axis.** Use the axis dropdown to choose a rotation reference:
    - An origin axis (X, Y, or Z) of the Body.
    - A datum line.
    - A sketch axis (N_Axis — the sketch normal — is the default when a
      sketch-based feature is selected).
    - A straight edge or a circular/arc edge (the axis of the arc is used).
    - Click **Select reference** to pick from the viewport.
   Use the **Reverse** button to flip the rotation direction (counter-clockwise
   → clockwise).

6. **Set the Mode:**
    - **Extent** — enter the total angular span in **Angle**. The pattern
      distributes copies evenly across that span.
    - **Spacing** — enter the angular gap between consecutive copies in
      **Spacing**.

7. **Set Occurrences** to the total number of instances (including the
   original). For a full bolt circle with 6 holes, set `Angle = 360°` and
   `Occurrences = 6`.

8. **Click OK.** FreeCAD builds the pattern and adds a PolarPattern feature to
   the model tree.

!!! tip
    For a full 360° pattern with `n` instances, the pattern uses an angular step
    of `360° / n` (not `360° / (n − 1)`), so the last copy does not coincide
    with the first. For any other angle, the step is `Angle / (n − 1)`.

---

## Parameters

### Common transformation controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Transform body / Transform tool shapes | Radio buttons | Transform tool shapes <!-- TODO: verify default --> | **Transform body**: the whole Body solid is patterned. **Transform tool shapes**: only listed features are patterned. |
| Feature list | Ordered list | — | Features whose solid contributions are patterned. |
| Recompute on change | Checkbox | checked | Triggers an immediate recompute on any parameter change. |

### Polar Pattern controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Axis | Dropdown / link | N_Axis of profile sketch (or Z origin axis) | The rotation axis reference. Accepts a sketch axis, a datum line, a straight or circular edge, or an origin axis. |
| Reverse | Button (toggle) | off | Reverses the rotation direction from counter-clockwise (default) to clockwise. |
| Mode | Dropdown | `Extent` | `Extent`: uses total angular span. `Spacing`: uses the angle between consecutive instances. |
| Angle | Angle | `360°` | Total angular span from first to last copy. Range: just above 0° to 360°. Active only in **Extent** mode. |
| Spacing | Angle | `120°` | Angular gap between consecutive copies. Active only in **Spacing** mode. |
| Occurrences | Integer (≥ 1) | `3` | Total number of instances, including the original. |

---

## Advanced usage

### Full 360° vs. partial patterns

When **Angle** is exactly 360°, the angular step per copy is `360° / Occurrences`
rather than `360° / (Occurrences − 1)`. This ensures the last copy does not
land on top of the first. For any angle other than 360°, the step is
`Angle / (Occurrences − 1)`.

### Custom per-instance angular spacing

In **Spacing** mode, individual angular gaps can be overridden through the
`Spacings` property (a float list, one entry per gap). Any entry equal to
`-1.0` falls back to the global **Spacing** value. The UI exposes this through
dynamic spacing rows: click the **+** button beside the Spacing spinbox to add
a per-gap override.

### Using a circular edge as the axis

If you select an arc or circle edge as the axis reference, FreeCAD extracts the
axis of that circle and uses it as the rotation axis. This is convenient when
patterning features around an existing bore without creating a separate datum
line.

### Polar pattern inside Multi-Transform

When used as a sub-transformation inside a [Multi-Transform](multi-transform.md),
Polar Pattern shows only its axis, direction, mode, angle/spacing, and
occurrence controls — the feature list and transform-scope radio buttons are
managed by the parent Multi-Transform.

---

## Common mistakes and pitfalls

!!! warning "Pattern angle can't be null"
    **Cause:** The computed per-step angle is zero — typically because
    **Occurrences** is `1` or the **Angle** is exactly `0`.  
    **Fix:** Set Occurrences to at least `2`. For a single copy at an offset
    angle, use `Occurrences = 2` and set Angle to the desired offset.

!!! warning "One or more instances are rejected (outside the body)"
    **Cause:** Some rotated copies do not overlap with the existing Body solid
    and cannot be fused or cut.  
    **Fix:** Ensure the original feature and all its rotated copies lie within
    the Body's material. Reduce the Angle range or reposition the original.

!!! warning "Rotation edge must be a straight line, circle, or arc of circle"
    **Cause:** The selected edge is a spline, ellipse, or other non-linear,
    non-circular curve.  
    **Fix:** Select a straight edge (whose direction becomes the axis), a
    circular edge (whose circle axis is used), a datum line, or an origin axis.

!!! warning "Unexpected rotation direction"
    **Cause:** The axis direction is defined by the selected reference's
    orientation, which may differ from the expected positive normal.  
    **Fix:** Use the **Reverse** button in the task panel to flip the rotation
    direction, or select a reference that naturally points the way you want.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign
import Sketcher
import Part
import math

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

# Base pad: 40 mm diameter cylinder, 10 mm tall
base_sketch = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sketch)
base_sketch.addGeometry(
    Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 20), False
)
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = base_sketch
pad.Length = 10.0
doc.recompute()

# Small hole at radius 15 mm from centre
hole_sketch = doc.addObject("Sketcher::SketchObject", "HoleSketch")
body.addObject(hole_sketch)
hole_sketch.AttachmentSupport = (pad, ["Face3"])  # TODO: verify face index
hole_sketch.MapMode = "FlatFace"
doc.recompute()
hole_sketch.addGeometry(
    Part.Circle(App.Vector(15, 0, 0), App.Vector(0, 0, 1), 2), False
)
doc.recompute()

pocket = doc.addObject("PartDesign::Pocket", "Pocket")
body.addObject(pocket)
pocket.Profile = hole_sketch
pocket.Length = 10.0
doc.recompute()

# Polar pattern: 6 holes at 60° intervals (full 360°)
pattern = doc.addObject("PartDesign::PolarPattern", "PolarPattern")
body.addObject(pattern)
pattern.Originals = [pocket]
origin = body.getOrigin()
pattern.Axis = (origin.getZ(), [""])   # Z origin axis
pattern.Angle = 360.0
pattern.Occurrences = 6
doc.recompute()

print(f"Instances: {pattern.Occurrences}")  # 6
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Originals` | `App::PropertyLinkList` | — | Features to be patterned. Inherited from `Transformed`. |
| `TransformMode` | `App::PropertyEnumeration` | `"Features"` | `"Features"` or `"Whole shape"`. Inherited from `Transformed`. |
| `Axis` | `App::PropertyLinkSub` | — | The rotation axis reference (sketch axis, datum line, edge, or origin axis). |
| `Reversed` | `App::PropertyBool` | `False` | Reverses the rotation direction. |
| `Mode` | `App::PropertyEnumeration` | `"Extent"` | `"Extent"` or `"Spacing"`. |
| `Angle` | `App::PropertyAngle` | `360.0°` | Total angular span in Extent mode. Minimum: just above 0°; maximum: 360°. |
| `Offset` | `App::PropertyAngle` | `120.0°` | Angular gap between copies in Spacing mode. |
| `Occurrences` | `App::PropertyIntegerConstraint` | `3` | Total number of instances, including the original (≥ 1). |
| `Spacings` | `App::PropertyFloatList` | `[-1.0, -1.0, -1.0]` | Per-gap angular overrides. `-1.0` means use global `Offset`. |
| `Refine` | `App::PropertyBool` | — | Remove redundant edges from the result. Inherited from `FeatureRefine`. |

---

## See also

- [Linear Pattern](linear-pattern.md) — repeat features along a straight line
- [Mirror](mirrored.md) — single reflection across a plane
- [Multi-Transform](multi-transform.md) — chain multiple transformation types in one feature
- [Pad](pad.md) — the additive feature most commonly used as the pattern input
- [Pocket](pocket.md) — the subtractive feature most commonly used as the pattern input
