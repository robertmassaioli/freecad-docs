# Linear Pattern

> **In one sentence:** Duplicate selected features in a straight-line array along one
> or two directions, controlled by a count and a total length or spacing.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Transformation Features → Linear Pattern  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Linear Pattern takes one or more existing Body features and repeats them at
regular (or explicitly defined) intervals along a direction vector. The copies
are fused into the Body exactly like the originals — additive features add
material, subtractive features cut it. A second independent direction can be
enabled to create a rectangular grid of copies in a single feature.

---

## Intuition

Imagine drilling a row of screw holes along the top of an enclosure. Instead of
creating ten separate pocket operations, you create one pocket and tell Linear
Pattern to repeat it nine more times with 20 mm between each copy. Change the
spacing to 25 mm and all ten holes instantly update.

The pattern operates on *features*, not on the whole Body. This means the
pattern copies only the selected pads, pockets, revolutions, etc., leaving any
surrounding geometry (base plate, walls, fillets) untouched.

The two sizing modes give different controls:

- **Extent** mode: you specify the total distance from the first copy to the
  last, and the pattern divides that evenly.
- **Spacing** mode: you specify the gap between consecutive copies, and the
  pattern extends as far as needed.

---

## When to use it

- You need a row of identical holes, bosses, slots, or any other feature spaced
  evenly along a straight line.
- You need a rectangular grid of copies (use both Direction 1 and Direction 2).
- You want the spacing or count to be parametric — driven by a named dimension
  or spreadsheet cell.
- The original feature is complicated (a swept pocket, a chamfered boss) and
  re-drawing it multiple times would be error-prone.

---

## When NOT to use it

- **The copies should be arranged around a central axis** — use
  [Polar Pattern](polar-pattern.md) instead.
- **You only need one reflected copy across a plane** — use
  [Mirror](mirrored.md); it is simpler to set up.
- **You need to combine a linear array with a polar array or a mirror in one
  step** — use [Multi-Transform](multi-transform.md).
- **The pattern direction follows a curve rather than a straight line** — there
  is no curved-path pattern in Part Design; consider a sketch-driven
  approach or multiple individual features instead.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one sketch-based feature to
pattern, and a reference for the direction — an origin axis, a datum line, a
sketch axis, a straight edge, or the normal of a planar face.

1. **Select the feature(s) to pattern.** Click one or more features in the
   model tree. You can also skip this step and add them inside the task panel.

2. **Activate Linear Pattern.** Go to **Part Design → Transformation Features
   → Linear Pattern**. The task panel opens.

3. **Choose transform scope.** Select either **Transform body** (mirror the
   whole Body solid) or **Transform tool shapes** (pattern only the listed
   features). For feature-level patterning, keep **Transform tool shapes**.

4. **Add features.** Use **Add Feature** / **Remove Feature** to populate the
   feature list.

5. **Set Direction 1.** In the **Direction 1** section, use the direction
   dropdown to choose a reference:
    - An origin axis (X, Y, or Z).
    - A datum line.
    - A sketch axis (H_Axis, V_Axis, or a named construction line).
    - A straight edge or the normal of a planar face — click **Select
      reference** and pick from the viewport.
   Use the **Reverse** button to flip the direction.

6. **Set the Mode** for Direction 1:
    - **Extent** — enter the total distance in the **Length** field. The pattern
      places copies at equal intervals spanning that total distance.
    - **Spacing** — enter the gap between consecutive copies in the **Spacing**
      field.

7. **Set Occurrences** to the total number of copies (including the original).
   A value of `2` produces the original plus one copy.

8. **Enable Direction 2 (optional).** Tick the **Direction 2** checkbox to add
   a second independent direction for a rectangular grid. Configure it the same
   way as Direction 1.

9. **Click OK.** FreeCAD builds the pattern and adds a LinearPattern feature to
   the model tree.

!!! tip
    Set **Recompute on change** on while still dialling in values so you can
    see the result update live. Uncheck it once the pattern is large (many
    copies of a complex feature) to avoid slow recomputes on every keystroke.

---

## Parameters

### Common transformation controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Transform body / Transform tool shapes | Radio buttons | Transform tool shapes <!-- TODO: verify default --> | **Transform body**: the whole Body solid is patterned. **Transform tool shapes**: only the listed features are patterned. |
| Feature list | Ordered list | — | Features whose solid contributions are patterned. Use **Add Feature** / **Remove Feature** to modify. |
| Recompute on change | Checkbox | checked | Triggers an immediate recompute on any parameter change. |

### Direction 1

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Direction | Dropdown / link | H_Axis of profile sketch (or X origin axis) | The vector along which the pattern is laid out. Accepts a sketch axis, a datum line, a straight edge of a feature, or the normal of a planar face. |
| Reverse | Button (toggle) | off | Reverses the direction of travel. |
| Mode | Dropdown | `Extent` | `Extent`: uses total distance from first to last copy. `Spacing`: uses per-step distance. |
| Length | Length | `100 mm` | Total distance from the first to the last copy. Active only when Mode is **Extent**. |
| Spacing | Length | `10 mm` | Distance between consecutive copies. Active only when Mode is **Spacing**. |
| Occurrences | Integer (≥ 1) | `2` | Total number of copies, including the original. |

### Direction 2 (optional)

Direction 2 is disabled by default (`Occurrences2 = 1`). Enable it by ticking
the **Direction 2** checkbox in the task panel.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Direction 2 | Dropdown / link | V_Axis of profile sketch (or none) | Second direction for a rectangular grid. Same reference options as Direction 1. |
| Reverse 2 | Button (toggle) | off | Reverses Direction 2. |
| Mode 2 | Dropdown | `Extent` | Same as Mode for Direction 1, applied to Direction 2. |
| Length 2 | Length | `100 mm` | Total length in Direction 2. Active only when Mode 2 is **Extent**. |
| Spacing 2 | Length | `10 mm` | Spacing between copies in Direction 2. Active only when Mode 2 is **Spacing**. |
| Occurrences 2 | Integer (≥ 1) | `1` | Total copies in Direction 2. Set to `1` (disabled) by default. |

---

## Advanced usage

### Custom per-instance spacing

In **Spacing** mode, individual copy intervals can be overridden through the
`Spacings` property (a float list with one entry per gap). Any entry set to
`-1.0` falls back to the global **Spacing** value. This is exposed in the UI
via the dynamic spacing rows (click the **+** button next to the Spacing
spinbox to add a custom value for a specific gap).

### Rectangular grid

When both directions are active, the total number of solid instances is
`Occurrences × Occurrences2`. All Direction 1 copies are placed for each
Direction 2 step, creating a full rectangular grid. There is no option to skip
individual grid positions — for that level of control, use separate features or
scripting.

### Direction from a face normal

If you select a planar face as the direction reference, Linear Pattern uses the
face's outward normal as the direction vector. This is useful for patterning
features along the face of an enclosure at an arbitrary angle without creating
a datum line.

---

## Common mistakes and pitfalls

!!! warning "One or more instances are rejected (outside the body)"
    **Cause:** Some copies of the original feature do not overlap with the
    existing Body solid, so they cannot be fused or cut.  
    **Fix:** Reduce the count, shorten the length, or reposition the original
    feature. The rejected shapes are stored in the `rejected` compound on the
    feature object and can be inspected in the Python console.

!!! warning "Pattern length too small"
    **Cause:** The **Length** value (in Extent mode) is smaller than
    `Precision::Confusion()` (effectively zero), so there is nowhere to place
    copies.  
    **Fix:** Enter a positive non-zero value for Length.

!!! warning "Direction edge must be a straight line"
    **Cause:** The selected edge is curved (an arc, circle, or spline).  
    **Fix:** Select a straight edge, a datum line, or an origin axis as the
    direction reference. Curved edges are not supported as pattern directions.

!!! warning "Second direction produces no copies"
    **Cause:** **Occurrences 2** is `1` (the default), so Direction 2 is
    effectively disabled.  
    **Fix:** Enable Direction 2 by ticking its checkbox in the task panel, or
    set `Occurrences2` to `2` or more in the Properties panel.

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

# Base pad: 60 × 20 × 5 mm plate
base_sketch = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sketch)
base_sketch.addGeometry(
    Part.Rectangle(App.Vector(0, 0, 0), App.Vector(60, 20, 0)), False
)
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = base_sketch
pad.Length = 5.0
doc.recompute()

# Hole pocket sketch
hole_sketch = doc.addObject("Sketcher::SketchObject", "HoleSketch")
body.addObject(hole_sketch)
hole_sketch.AttachmentSupport = (pad, ["Face6"])  # TODO: verify face index
hole_sketch.MapMode = "FlatFace"
doc.recompute()
hole_sketch.addGeometry(
    Part.Circle(App.Vector(5, 10, 0), App.Vector(0, 0, 1), 2), False
)
doc.recompute()

pocket = doc.addObject("PartDesign::Pocket", "Pocket")
body.addObject(pocket)
pocket.Profile = hole_sketch
pocket.Length = 5.0
doc.recompute()

# Linear pattern: 4 holes, 15 mm apart in X
pattern = doc.addObject("PartDesign::LinearPattern", "LinearPattern")
body.addObject(pattern)
pattern.Originals = [pocket]
origin = body.getOrigin()
pattern.Direction = (origin.getX(), [""])  # X axis
pattern.Mode = "Spacing"
pattern.Offset = 15.0       # 15 mm between each hole
pattern.Occurrences = 4     # original + 3 copies
doc.recompute()

print(f"Instances: {pattern.Occurrences}")  # 4
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Originals` | `App::PropertyLinkList` | — | Features to be patterned. Inherited from `Transformed`. |
| `TransformMode` | `App::PropertyEnumeration` | `"Features"` | `"Features"` or `"Whole shape"`. Inherited from `Transformed`. |
| `Direction` | `App::PropertyLinkSub` | — | Reference for the first pattern direction (sketch axis, datum line, edge, or face normal). |
| `Reversed` | `App::PropertyBool` | `False` | Reverses Direction 1. |
| `Mode` | `App::PropertyEnumeration` | `"Extent"` | Sizing mode for Direction 1: `"Extent"` or `"Spacing"`. |
| `Length` | `App::PropertyLength` | `100.0` | Total extent in Direction 1 (Extent mode). |
| `Offset` | `App::PropertyLength` | `10.0` | Per-step spacing in Direction 1 (Spacing mode). |
| `Occurrences` | `App::PropertyIntegerConstraint` | `2` | Number of copies in Direction 1 (≥ 1, includes original). |
| `Spacings` | `App::PropertyFloatList` | `[-1.0]` | Per-gap overrides for Direction 1. `-1.0` means use global `Offset`. |
| `Direction2` | `App::PropertyLinkSub` | — | Reference for the optional second direction. |
| `Reversed2` | `App::PropertyBool` | `False` | Reverses Direction 2. |
| `Mode2` | `App::PropertyEnumeration` | `"Extent"` | Sizing mode for Direction 2. |
| `Length2` | `App::PropertyLength` | `100.0` | Total extent in Direction 2 (Extent mode). |
| `Offset2` | `App::PropertyLength` | `10.0` | Per-step spacing in Direction 2 (Spacing mode). |
| `Occurrences2` | `App::PropertyIntegerConstraint` | `1` | Number of copies in Direction 2 (1 = disabled). |
| `Spacings2` | `App::PropertyFloatList` | `[]` | Per-gap overrides for Direction 2. |
| `Refine` | `App::PropertyBool` | — | Remove redundant edges from the result. Inherited from `FeatureRefine`. |

---

## See also

- [Polar Pattern](polar-pattern.md) — circular repetition around an axis
- [Mirror](mirrored.md) — single reflection across a plane
- [Multi-Transform](multi-transform.md) — combine linear, polar, and mirror transformations
- [Pad](pad.md) — the additive feature most commonly used as the pattern input
- [Pocket](pocket.md) — the subtractive feature most commonly used as the pattern input
