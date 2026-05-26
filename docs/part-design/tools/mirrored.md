# Mirror

> **In one sentence:** Mirror selected features across a plane to create a symmetrical
> copy without duplicating sketches or parameters.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Transformation Features → Mirror  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Mirror takes one or more existing Body features and creates a reflected copy of
each across a chosen plane. The result is fused back into the Body — it adds or
removes material exactly as the originals do, just on the opposite side of the
mirror plane. Because it references the originals parametrically, any change to
the original feature is automatically reflected in the mirrored copy.

---

## Intuition

Think of placing a pocket or boss next to a mirror: you see the original on one
side and its exact reflection on the other, perfectly equidistant from the
glass. The "glass" is the mirror plane — a flat reference that Mirror uses to
flip the geometry.

The key point is that Mirror operates on *features*, not on the entire Body.
You choose which features to mirror (e.g. a single countersunk hole) and Mirror
creates reflected copies of only those features, leaving the rest of the Body
unchanged. If you want to mirror everything, you either select all features or
use the **Transform body** mode in the task panel.

---

## When to use it

- Your part is symmetric and you have modelled one half: mirror the additive and
  subtractive features to complete the other half without re-drawing them.
- You need a hole, boss, or slot to appear symmetrically on both sides of a
  datum plane (e.g. two mounting holes either side of a centreline).
- You want to enforce symmetry parametrically — if the original feature moves,
  the mirrored copy follows.
- You are working inside a MultiTransform and want one stage of the chain to be
  a reflection.

---

## When NOT to use it

- **You need more than two copies, or copies arranged in a line** — use
  [Linear Pattern](linear-pattern.md) instead.
- **You need copies arranged around an axis** — use
  [Polar Pattern](polar-pattern.md) instead.
- **You want to combine several kinds of transformation in one operation** — use
  [Multi-Transform](multi-transform.md) instead.
- **The part itself (not individual features) needs to be mirrored as a solid
  operation** — use Part → Mirror from the Part workbench.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body containing at least one sketch-based feature
(Pad, Pocket, Revolution, etc.) and a suitable mirror plane — either the Body's
built-in origin planes (XY, XZ, YZ), a Datum Plane, or a planar face of the
solid.

1. **Select the feature(s) to mirror.** Click one or more features in the model
   tree or 3D view. You can also make no selection and add features inside the
   task panel.

2. **Activate Mirror.** Go to **Part Design → Transformation Features → Mirror**.
   The task panel opens.

3. **Choose the transform scope.** At the top of the task panel two radio
   buttons appear:
    - **Transform body** — the transformation is applied to the whole Body shape.
    - **Transform tool shapes** — the transformation is applied only to the listed
      features (individual pads, pockets, etc.).  
   Select **Transform tool shapes** for feature-level mirroring (the default for
   most cases).

4. **Add or remove features in the list.** Use **Add Feature** and **Remove
   Feature** to populate the feature list with the features you want mirrored.
   The list shows the features' labels.

5. **Select the mirror plane.** In the **Plane** dropdown, choose:
    - An origin plane (XY, XZ, or YZ) of the Body.
    - A Datum Plane you created earlier.
    - A planar face on the existing solid (click **Select reference** and then
      pick a face in the viewport).
    - A sketch axis (horizontal, vertical, or a named construction axis).

6. **Click OK.** FreeCAD computes the mirror and adds a Mirrored feature to the
   model tree.

!!! tip
    If the mirrored copy causes a BREP validation failure (e.g. the original and
    its reflection are tangent or nearly overlapping), try using a face that is
    clearly offset from both features. Mirror requires that the reflected copy
    does not coincide with the original.

---

## Parameters

The common header section (shared by all transformation features) controls which
features are transformed. The Mirror-specific section controls the plane.

### Common transformation controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Transform body / Transform tool shapes | Radio buttons | Transform tool shapes <!-- TODO: verify default --> | When **Transform body** is selected (`TransformMode = "Whole shape"`), the entire Body solid is mirrored. When **Transform tool shapes** is selected (`TransformMode = "Features"`), only the listed features are mirrored. |
| Feature list | Ordered list | — | The features whose solid contributions (add or subtract) will be mirrored. Use **Add Feature** / **Remove Feature** buttons to modify the list. |
| Recompute on change | Checkbox | checked | Triggers an immediate recompute whenever a parameter changes. Uncheck for large models to defer recomputation until OK. |

### Mirror-specific controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Plane | Dropdown / link | V_Axis of profile sketch (or XY origin plane) | The plane across which features are reflected. Accepts a Datum Plane, an origin plane (XY / XZ / YZ), a planar face of the solid, or a sketch axis. |

---

## Advanced usage

### Mirroring inside Multi-Transform

When Mirrored is used as a sub-transformation inside a
[Multi-Transform](multi-transform.md), it does not show the feature list or the
transform-scope radio buttons — those are controlled by the parent
Multi-Transform. Only the **Plane** dropdown is shown.

### Using a sketch axis as the plane

When the profile sketch is available (i.e. the original feature is
sketch-based), you can select the sketch's horizontal axis (**H_Axis**) or
vertical axis (**V_Axis**) as the mirror reference. The mirror plane is then the
plane that contains that axis and is perpendicular to the sketch plane. Named
construction lines in the sketch appear as **Axis0**, **Axis1**, etc.

---

## Common mistakes and pitfalls

!!! warning "Mirror fails with 'could not be created' error"
    **Cause:** The mirrored copy overlaps or is coincident with the original,
    producing a degenerate OCCT shape.  
    **Fix:** Check that the mirror plane does not pass through the original
    feature. If it does, offset the plane or redesign the feature so that the
    reflected copy is cleanly separated.

!!! warning "No plane reference specified error"
    **Cause:** The **Plane** dropdown is blank — typically because the task
    panel was opened without a valid sketch or origin in the Body.  
    **Fix:** Make sure the Body has an origin (all Bodies created via **Part
    Design → Create Body** do). Re-open the task panel and pick a plane from the
    dropdown.

!!! warning "Wrong side is mirrored"
    **Cause:** The mirror plane normal points in the opposite direction from what
    was expected.  
    **Fix:** Mirror has no **Reversed** option. Change the reference plane or
    reposition the original feature so it ends up on the correct side after
    reflection.

!!! warning "Mirrored feature disappears in the model tree"
    **Cause:** Mirror operates on features, not standalone objects. If the
    selected feature is removed from the Body, the Mirror feature will fail on
    the next recompute.  
    **Fix:** Keep the original feature in the Body and above the Mirror feature
    in the feature tree (as a dependency).

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign
import Sketcher
import Part

doc = App.newDocument()

# Create the Body
body = doc.addObject("PartDesign::Body", "Body")

# Profile sketch on XY plane
sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
body.addObject(sketch)
sketch.addGeometry(Part.Rectangle(App.Vector(-5, 0, 0), App.Vector(5, 10, 0)), False)
doc.recompute()

# Pad the sketch 5 mm
pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = sketch
pad.Length = 5.0
doc.recompute()

# Mirror the Pad across the YZ plane (Body origin)
mirrored = doc.addObject("PartDesign::Mirrored", "Mirrored")
body.addObject(mirrored)
mirrored.Originals = [pad]
origin = body.getOrigin()
mirrored.MirrorPlane = (origin.getYZ(), [""])  # TODO: verify sub-element string
doc.recompute()
```

### Method signature

```python
doc.addObject("PartDesign::Mirrored", "<name>")
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Originals` | `App::PropertyLinkList` | The features to be mirrored. Inherited from `Transformed`. |
| `TransformMode` | `App::PropertyEnumeration` | `"Features"` mirrors only listed features; `"Whole shape"` mirrors the entire Body solid. Inherited from `Transformed`. |
| `MirrorPlane` | `App::PropertyLinkSub` | The plane reference. Can point to a `PartDesign::Plane`, `App::Plane` (origin plane), a planar `Part::Feature` face, or a `Part::Part2DObject` axis sub-element. |
| `Refine` | `App::PropertyBool` | When `True`, removes redundant edges from the resulting shape. Inherited from `FeatureRefine`. |

---

## See also

- [Linear Pattern](linear-pattern.md) — repeat features along a straight line
- [Polar Pattern](polar-pattern.md) — repeat features around an axis
- [Multi-Transform](multi-transform.md) — chain multiple transformations in one feature
- [Datum Plane](../concepts/body-and-origin.md) — how to create datum planes for use as mirror references
