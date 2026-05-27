# Multi-Transform

> **In one sentence:** Apply multiple transformation steps (mirror, linear pattern, polar pattern, scale) to the same set of features in sequence, producing a compound transformation from a single feature.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Apply a pattern → Multi-Transform  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Multi-Transform chains a list of child transformation features — Mirrored, LinearPattern, PolarPattern, and Scaled — into a single compound transformation. Each child transformation is applied to the result of the previous one. The final set of solid instances is fused back into the Body just like any other pattern feature: additive originals add material, subtractive originals remove it.

The child transformations are created and managed directly inside the Multi-Transform task panel. They do not appear as independent usable features in the model tree; they exist solely to define the steps of the compound transformation.

---

## Intuition

Imagine you need to model the bolt-hole pattern on a circular flange where the holes are arranged in a ring and the ring itself is mirrored across the flange centreline. With individual pattern tools you would need two separate features. With Multi-Transform you add one Polar Pattern step (for the ring) and one Mirrored step (for the top/bottom symmetry) inside a single feature. Change the number of holes in the Polar Pattern, and the mirrored ring updates automatically.

Another classic example: a gear ring that must be both rotated (Polar Pattern, N teeth) and scaled (Scaled, making teeth taper). Combining these in Multi-Transform keeps the model tree short and the relationship explicit.

The mathematical rule for combining transformations is:

- **Non-Scaled steps** use multiplication: every transformation from step N is combined with every transformation from step N+1, so the total count multiplies. Two steps of 3 and 4 occurrences produce 3 × 4 = 12 instances.
- **Scaled steps** use the diagonal method: the scale factor at position *i* is applied to instance *i* of the previous step. The Scaled occurrence count must divide evenly into the previous step's count.

---

## When to use it

- You need to combine a mirror and a linear or polar array in a single parametric step.
- You want to produce a ring-of-rings, a grid-plus-mirror, or any other nested pattern.
- You want to apply a scale gradient across an existing set of pattern instances.
- Keeping the model tree clean matters: one Multi-Transform is easier to read than three separate pattern features chained together.

---

## When NOT to use it

- **You only need a single linear array** — use [Linear Pattern](linear-pattern.md); it is simpler to configure.
- **You only need a single polar array** — use [Polar Pattern](polar-pattern.md).
- **You only need a single mirror** — use [Mirrored](mirrored.md).
- **The transformations are independent and should be visible as separate features** — keep them as separate pattern features so they can be individually suppressed or reordered.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one sketch-based feature (Pad, Pocket, Revolution, etc.) to pattern. Know in advance which transformation types you want to stack and in what order.

1. **Select the feature(s) to transform.** Click one or more features in the model tree. You can also add them inside the task panel.

2. **Activate Multi-Transform.** Go to **Part Design → Apply a pattern → Multi-Transform**. The task panel opens with an empty **Transformations** list.

3. **Add features to transform.** If you did not pre-select, use **Add Feature** / **Remove Feature** to populate the list of originals at the top of the task panel.

4. **Add the first child transformation.** Right-click inside the Transformations list to open the context menu. Choose one of:
    - **Add Mirror Transformation** — adds a Mirrored child.
    - **Add Linear Pattern** — adds a LinearPattern child with default Length 100 mm and 2 occurrences.
    - **Add Polar Pattern** — adds a PolarPattern child with default Angle 360° and 2 occurrences.
    - **Add Scale Transformation** — adds a Scaled child with default Factor 2 and 2 occurrences.

5. **Configure the child transformation.** After adding, the child's parameter panel appears inline in the task panel (the same controls as when editing the child type standalone). Set the axis, plane, length, angle, occurrences, etc.

6. **Click the sub-task OK button** to close the inline editor and return to the Transformations list.

7. **Repeat for additional steps.** Add more child transformations as needed. Use **Move Up** / **Move Down** (right-click context menu) to reorder them.

8. **Edit an existing child.** Double-click any entry in the Transformations list (or right-click → **Edit**) to re-open its inline parameter editor.

9. **Remove a child.** Right-click → **Delete** removes the selected child transformation from the list and from the document.

10. **Click OK** in the outer task panel. FreeCAD recomputes all transformation steps and adds the MultiTransform feature to the model tree.

!!! tip
    When a new child is added, its direction or plane defaults to the sketch's H_Axis (or the body's origin X axis if no sketch is available). Change this immediately before closing the sub-task to avoid an extra recompute cycle.

!!! tip
    If the total instance count is unexpectedly large, check the order of steps. Non-Scaled steps multiply: two LinearPattern steps with 5 and 4 occurrences give 20 instances. Reordering does not change the count but may affect which instances land inside the Body, which can affect which ones are accepted or rejected.

---

## Parameters

### Outer Multi-Transform controls

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Transform body / Transform tool shapes | Radio buttons | Transform tool shapes | **Transform body**: the whole Body solid is transformed. **Transform tool shapes**: only the listed features are transformed. |
| Feature list (Originals) | Ordered list | — | Features whose solid contributions are to be transformed. Use **Add Feature** / **Remove Feature** to modify. |
| Transformations list | Ordered list | — | The sequence of child transformation features (Mirrored, LinearPattern, PolarPattern, Scaled) applied in order. |

### Child transformation parameters

Each child has its own parameters, identical to those of the corresponding standalone tool:

| Child type | Key parameters |
|------------|----------------|
| Mirrored | Plane reference (Datum Plane, origin plane, planar face, or sketch axis) |
| LinearPattern | Direction, Mode (Extent / Spacing), Length or Offset, Occurrences |
| PolarPattern | Axis, Angle, Occurrences |
| Scaled | Factor (uniform scale), Occurrences |

See [Mirrored](mirrored.md), [Linear Pattern](linear-pattern.md), [Polar Pattern](polar-pattern.md) for full parameter details.

---

## Advanced usage

### Multiplication vs. diagonal combination

When two or more non-Scaled steps are chained, the total instance count is the **product** of all occurrence counts. Example: Mirrored (2 instances) → PolarPattern (6 occurrences) produces 2 × 6 = 12 instances.

When a Scaled step is added, it uses the **diagonal** method instead. The Scaled step does not multiply the count — it must have an occurrence count that divides evenly into the current total. Each scale factor in the Scaled step is applied to the corresponding slice of existing instances. This is useful for modelling tapered teeth or fan blades that both repeat and grow.

### Combining mirror + polar for bolt-ring patterns

A common pattern for bolt holes on a flange:

1. Create one Pocket (the hole) at the desired bolt-circle radius.
2. Add Multi-Transform.
3. Add a PolarPattern child: axis = Body Z axis, Angle = 360°, Occurrences = number of holes.
4. Optionally add a Mirrored child if the flange has a second bolt circle on the other side.

This produces the full bolt pattern in one feature.

### Reordering steps changes the result

Because each step feeds into the next, reordering steps can produce a different set of instance positions — especially when the steps involve non-commutative transformations (e.g. a translation followed by a rotation versus a rotation followed by a translation). Use **Move Up** / **Move Down** to experiment.

---

## Common mistakes and pitfalls

!!! warning "One or more instances are rejected"
    **Cause:** Some instances of the patterned feature land outside the existing Body solid and cannot be fused or cut.  
    **Fix:** Reduce occurrence counts, shorten linear extents, or reposition the original feature so all instances land within the Body.

!!! warning "Number of occurrences must be a divisor of previous number of occurrences"
    **Cause:** A Scaled child step has an occurrence count that does not evenly divide the total count from the previous steps.  
    **Fix:** Ensure the Scaled step's occurrence count is a factor of the total instance count of all preceding steps. For example, if preceding steps produce 12 instances, the Scaled step can have 1, 2, 3, 4, 6, or 12 occurrences.

!!! warning "Empty Transformations list — feature produces the original only"
    **Cause:** The Transformations list is empty (all child steps were deleted or none were added).  
    **Fix:** Add at least one child transformation via the right-click context menu. An empty list is silent — the feature will execute and produce a shape identical to the original without any transformation applied.

!!! warning "Sub-task parameter changes do not appear to take effect"
    **Cause:** The inline sub-task editor is still open. Changes to child parameters are not committed to the parent Multi-Transform until you click the sub-task's **OK** button.  
    **Fix:** Always click the inline **OK** button to close the sub-task before clicking the outer task panel's **OK**.

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

# Base pad: 10 x 10 x 10 mm cube
sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
body.addObject(sketch)
sketch.addGeometry(Part.Rectangle(App.Vector(0, 0, 0), App.Vector(10, 10, 0)), False)
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = sketch
pad.Length = 10.0
doc.recompute()

# Create the MultiTransform
mt = doc.addObject("PartDesign::MultiTransform", "MultiTransform")
body.addObject(mt)
mt.Originals = [pad]
doc.recompute()

# Add a Mirrored sub-transformation
mirrored = body.newObject("PartDesign::Mirrored", "Mirror")
mirrored.MirrorPlane = (sketch, ["H_Axis"])
doc.recompute()

# Add a LinearPattern sub-transformation
lp = body.newObject("PartDesign::LinearPattern", "LinearPattern")
lp.Direction = (sketch, ["H_Axis"])
lp.Length = 20.0
lp.Occurrences = 3
doc.recompute()

# Wire them into the MultiTransform
mt.Transformations = [mirrored, lp]
doc.recompute()

print(f"MultiTransform shape volume: {mt.Shape.Volume}")
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Originals` | `App::PropertyLinkList` | — | Features to be transformed. Inherited from `Transformed`. |
| `Transformations` | `App::PropertyLinkList` | `[]` | Ordered list of child transformation objects (Mirrored, LinearPattern, PolarPattern, Scaled). |
| `TransformMode` | `App::PropertyEnumeration` | `"Features"` | `"Features"` or `"Whole shape"`. Inherited from `Transformed`. |
| `Refine` | `App::PropertyBool` | `False` | Remove redundant edges from the final shape. Inherited from `FeatureRefine`. |

---

## See also

- [Mirrored](mirrored.md) — single reflection across a plane
- [Linear Pattern](linear-pattern.md) — repeat features along a straight line
- [Polar Pattern](polar-pattern.md) — repeat features around an axis
- [Pad](pad.md) — additive feature most commonly used as the pattern input
- [Pocket](pocket.md) — subtractive feature most commonly used as the pattern input
