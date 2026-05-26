# Subtractive Box

> **In one sentence:** Cut a rectangular box-shaped cavity out of an existing solid in a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Box  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveSubtractive) · **Shortcut:** none

Subtractive Box creates a parametric rectangular cuboid — defined by three independent dimensions (Length, Width, Height) — and subtracts it from the active Body's current solid using a boolean cut. It is the subtractive counterpart of [Additive Box](additive-box.md) and shares identical parameters. No sketch is required: the box is placed and sized immediately through a task panel.

---

## Intuition

Think of pressing a rectangular cookie-cutter through a block of clay and pulling out the cut piece. The solid block (the Body's existing geometry) remains, but a rectangular void now runs through it at exactly the size and position you specified.

Because no sketch is involved, you skip the usual Sketcher round-trip. This makes Subtractive Box the fastest way to introduce a rectangular recess, slot, pocket, or through-hole when the cross-section is a simple rectangle and you already have a solid to cut into.

The box is placed relative to the Body's local coordinate system by default, with Length along X, Width along Y, and Height along Z. An **Attachment** section lets you reposition it to any face, datum plane, or existing geometry.

---

## When to use it

- You want to cut a rectangular slot, notch, or recess into an existing solid without drawing a sketch.
- You need a box-shaped through-hole or blind pocket whose three dimensions should be parametrically independent (not tied to a sketch profile).
- You are scripting or templating models and want a single API call to produce a rectangular cut.
- You are doing rapid concept modelling and want to remove box-shaped material before committing to sketch-based features.
- You want the cut dimensions driven directly by expressions from a spreadsheet, without going through sketch constraints.

---

## When NOT to use it

- **The cross-section to remove is not a simple rectangle** — draw the profile as a sketch and use [Pocket](pocket.md) instead. Any non-rectangular outline (rounded corners, chamfers, L-shapes) requires a sketch.
- **You need a sketch-based cut for tight integration with the feature tree** — use [Pocket](pocket.md), which is more flexible and better supported by pattern tools (Mirrored, Polar Pattern, etc.).
- **You want to add material in a box shape** — use [Additive Box](additive-box.md), which has identical parameters but fuses instead of cuts.
- **No solid exists in the Body yet** — a subtractive feature requires an existing base feature to subtract from. Create at least one additive feature first.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one existing additive feature (the solid to cut into). If the Body has no tip feature, FreeCAD will refuse to create the subtractive primitive.

1. **Activate Part Design.** Open or switch to the Part Design workbench.

2. **Invoke the tool.** Go to **Part Design → Subtractive Primitives → Subtractive Box**. Alternatively, click the Subtractive Box icon in the toolbar (inside the CompPrimitiveSubtractive drop-down button).

3. **The task panel opens** showing three input fields: Length, Width, and Height. All three default to 10 mm. The 3-D view shows the box as a preview shape overlaid on the existing solid.

4. **Set the dimensions.** Type new values or use the spin box arrows. The preview updates live.

5. **Optionally set an attachment.** The task panel includes an **Attachment** section at the bottom. Use it to align the box to an existing face, edge, vertex, or datum plane so the cut lands where you need it.

6. **Click OK.** The box is subtracted from the Body and appears as `Box` (or a unique name like `Box001`) in the model tree.

!!! tip
    Double-click the Box entry in the model tree at any time to reopen the task panel and change any dimension. All downstream features will recompute.

!!! warning "No base feature available"
    If the Body has no tip feature when you invoke the tool, FreeCAD shows a warning and cancels. Add at least one additive feature (e.g. [Additive Box](additive-box.md)) first, then apply the Subtractive Box on top.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Length | Length (mm) | 10.0 mm | Dimension of the cut along the Body's local X-axis. Must be greater than zero. |
| Width | Length (mm) | 10.0 mm | Dimension of the cut along the Body's local Y-axis. Must be greater than zero. |
| Height | Length (mm) | 10.0 mm | Dimension of the cut along the Body's local Z-axis. Must be greater than zero. |

The task panel also exposes the **Attachment** section (inherited from `Part::AttachExtension`), which lets you map the box origin to a face, edge, vertex, or datum object. See the [Attachment](../concepts/attachment.md) documentation for the full list of attachment modes. <!-- TODO: verify link -->

---

## Advanced usage

### Driving dimensions with expressions

Any of the three dimension properties accepts a FreeCAD expression. Right-click a dimension field in the task panel and select **Set expression** to bind the value to a spreadsheet cell or a named constraint. This is useful for cuts whose size must stay proportional to other model dimensions.

### Attaching to an existing face

Open the **Attachment** section at the bottom of the task panel and click **Attached to** to pick a face or datum plane. Use **Attachment offset** to fine-tune the position. A common pattern is to attach the Subtractive Box to the top face of an Additive Box to produce a centred pocket.

### Combining primitives for rapid concept modelling

A quick workflow for enclosure-style parts: start with an [Additive Box](additive-box.md) for the outer shell, then stack one or more Subtractive Boxes to hollow out interior volumes, cut mounting slots, and add ventilation openings — all without touching the Sketcher.

---

## Common mistakes and pitfalls

!!! warning "Cut appears at the wrong location"
    **Cause:** The box is placed relative to the Body origin by default. If the origin is far from the face you intended to cut, the subtraction happens in an unexpected location.  
    **Fix:** Use the **Attachment** section in the task panel to attach the box to the desired face or datum plane before clicking OK.

!!! warning "Cannot create the box — dimensions too small"
    **Cause:** FreeCAD enforces a minimum positive dimension. Setting Length, Width, or Height to zero or a very small number triggers an error (`Length of box too small`, etc.).  
    **Fix:** Ensure all three dimensions are greater than zero. The enforced minimum is `2 × Precision::Confusion()` (approximately 2 × 10⁻⁷ mm).

!!! warning "Resulting shape is not a solid"
    **Cause:** The boolean cut produced a degenerate result — for example, the box aligns exactly with a face of the base solid or the subtraction removes the entire solid.  
    **Fix:** Adjust the dimensions or position so the cutting box intersects the existing solid rather than being flush with or fully outside it.

!!! warning "Feature is refused — no base feature"
    **Cause:** Subtractive primitives require an existing solid in the Body to cut into. The tool checks `Body.Tip` before proceeding.  
    **Fix:** Create at least one additive feature (e.g. Additive Box) first, then add the Subtractive Box on top.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401 — registers PartDesign object types

doc = App.newDocument()

# Create a Body with an additive box to cut into
body = doc.addObject("PartDesign::Body", "Body")
base = doc.addObject("PartDesign::AdditiveBox", "Base")
body.addObject(base)
base.Length = 50.0
base.Width  = 40.0
base.Height = 20.0
doc.recompute()

# Create a Subtractive Box inside the Body
cut = doc.addObject("PartDesign::SubtractiveBox", "Pocket")
body.addObject(cut)

# Set dimensions of the cut
cut.Length = 20.0  # mm
cut.Width  = 15.0  # mm
cut.Height = 20.0  # mm  (full depth — cuts through)

doc.recompute()

print(f"Volume after cut: {cut.Shape.Volume:.4f} mm³")
# Expected: 50×40×20 − 20×15×20 = 40000 − 6000 = 34000.0000 mm³
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Length` | `App::PropertyLength` | `10.0 mm` | Dimension along local X. Must be > 0. |
| `Width` | `App::PropertyLength` | `10.0 mm` | Dimension along local Y. Must be > 0. |
| `Height` | `App::PropertyLength` | `10.0 mm` | Dimension along local Z. Must be > 0. |

Properties inherited from `PartDesign::FeatureAddSub` (available on all additive/subtractive features):

| Property | Type | Description |
|----------|------|-------------|
| `BaseFeature` | `App::PropertyLink` | The feature this cut is applied to. |
| `AddSubShape` | `Part::PropertyPartShape` | The raw box shape before the boolean cut. Useful for inspection. |

---

## See also

- [Additive Box](additive-box.md) — identical parameters, adds material instead of removing it
- [Pocket](pocket.md) — sketch-based rectangular or arbitrary-profile cut; better for complex shapes and patterns
- [Subtractive Cylinder](subtractive-cylinder.md) — cylindrical cut primitive
- [Subtractive Sphere](subtractive-sphere.md) — spherical cut primitive
- [Subtractive Cone](subtractive-cone.md) — conical/frustum cut primitive
