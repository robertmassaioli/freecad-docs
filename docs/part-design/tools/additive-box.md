# Additive Box

> **In one sentence:** Insert a rectangular solid block directly into a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Box  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveAdditive) · **Shortcut:** none

Additive Box creates a parametric rectangular cuboid — defined by three independent dimensions (Length, Width, Height) — and fuses it into the active Body. Unlike Pad, it does not require a pre-drawn sketch: the box is created immediately and then edited through a task panel. The result is a feature in the model tree just like any other additive feature.

---

## Intuition

Think of stamping a solid brick of clay into your model. You pick the three dimensions and the brick appears, fused with whatever material was already in the Body. Changing a dimension later is just a matter of double-clicking the feature and typing a new value.

The box is placed relative to the Body's local coordinate system (or to an attached datum plane if you set one). The corner of the box sits at the origin of that coordinate system by default, with Length along X, Width along Y, and Height along Z.

Because there is no sketch, you skip the usual Sketcher round-trip. This makes Additive Box the fastest path to a simple rectilinear solid, and also makes it easy to explore rough proportions before committing to a sketch-based design.

---

## When to use it

- You are starting a new Body and need a plain rectangular base to work from.
- You want to add a box-shaped boss, pad, or protrusion without drawing a rectangle sketch first.
- You are scripting or templating models and want a single API call to produce a box feature.
- You are prototyping proportions quickly and a sketch would add unnecessary ceremony.
- You need a box whose dimensions must be driven by expressions from the start, without going through sketch constraints.

---

## When NOT to use it

- **The box cross-section is not a simple rectangle** — draw the profile as a sketch and use [Pad](pad.md) instead. Any non-rectangular outline (rounded corners, chamfers, complex shapes) requires a sketch.
- **The box must follow a pattern or polar array** — use [Pad](pad.md) with a fully constrained sketch, which integrates more naturally with the Part Design pattern tools.
- **You need to remove material in a box shape** — use [Subtractive Box](subtractive-box.md), which has identical parameters but cuts instead of adds.
- **The box needs to be placed relative to an existing face by its face geometry** — consider [Pad](pad.md) with a sketch mapped to that face; the attachment system for primitives works but can be less intuitive than sketch attachment modes.

---

## Step-by-step walkthrough

**Prerequisite:** A document must be open. An active Body is required; if none exists, FreeCAD will offer to create one automatically.

1. **Activate Part Design.** Open or switch to the Part Design workbench.

2. **Invoke the tool.** Go to **Part Design → Additive Primitives → Additive Box**. Alternatively, click the Additive Box icon in the toolbar (it lives inside the CompPrimitiveAdditive drop-down button).

3. **If no Body exists,** FreeCAD will create one automatically. If multiple Bodies exist, a dialog asks you to choose which Body to work in.

4. **The task panel opens** showing three input fields: Length, Width, and Height. All three default to 10 mm.

5. **Set the dimensions.** Type new values or use the spin box arrows to change Length, Width, and Height. The 3-D view updates live as you change values.

6. **Optionally set an attachment.** The task panel includes an **Attachment** section at the bottom. Use it to attach the box to an existing face, datum plane, or coordinate system, if the default origin placement is not where you want the box.

7. **Click OK.** The box is fused into the Body and appears as `Box` (or a unique name like `Box001`) in the model tree.

!!! tip
    Double-click the Box entry in the model tree at any time to reopen the task panel and change any dimension. All downstream features that depend on the Body will recompute.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Length | Length (mm) | 10.0 mm | Dimension along the Body's local X-axis. Must be greater than zero. |
| Width | Length (mm) | 10.0 mm | Dimension along the Body's local Y-axis. Must be greater than zero. |
| Height | Length (mm) | 10.0 mm | Dimension along the Body's local Z-axis. Must be greater than zero. |

The task panel also exposes the **Attachment** section (inherited from `Part::AttachExtension`), which lets you map the box origin to a face, edge, vertex, or datum object. See the [Attachment](../concepts/attachment.md) documentation for the full list of attachment modes. <!-- TODO: verify link -->

---

## Advanced usage

### Driving dimensions with expressions

Any of the three dimension properties accepts a FreeCAD expression. Right-click a dimension field in the task panel and select **Set expression** to bind the value to a spreadsheet cell or a named constraint. This makes it straightforward to build fully parametric models where the box size is derived from a master parameter table.

### Attaching to an existing face

Open the **Attachment** section at the bottom of the task panel and click **Attached to** to pick a face or datum plane. Use **Attachment offset** to fine-tune the position. This is equivalent to mapping a sketch to a face, but for primitive features.

### Combining with subtractive primitives

A common quick-modelling pattern is to start with an Additive Box for the outer envelope, then cut away material with [Subtractive Box](subtractive-box.md), [Subtractive Cylinder](subtractive-cylinder.md), or similar tools, before switching to sketch-based features for fine details.

---

## Common mistakes and pitfalls

!!! warning "Box appears at the wrong location"
    **Cause:** The box is placed relative to the Body origin by default. If your Body origin is far from where you expected the box, the feature appears elsewhere.  
    **Fix:** Use the **Attachment** section in the task panel to attach the box to the desired face or datum plane, or move the Body's origin.

!!! warning "Cannot create the box — dimensions too small"
    **Cause:** FreeCAD enforces a minimum positive dimension. Setting Length, Width, or Height to zero or a very small value triggers an error (`Length of box too small`, etc.).  
    **Fix:** Ensure all three dimensions are greater than zero. The minimum enforced by the source is `2 × Precision::Confusion()` (approximately 2 × 10⁻⁷ mm).

!!! warning "Feature is not fused — result looks like a standalone solid"
    **Cause:** The Additive Box was created without a base feature in the Body, so there is nothing to fuse with. The box becomes the first (and so far only) solid in the Body, which is correct behaviour — it is not an error.  
    **Fix:** This is expected when the box is the first feature. Add further features (Pad, Pocket, other primitives) on top of it as normal.

!!! warning "Subtractive Box refused to create — no base feature"
    **Cause:** Subtractive primitives require an existing solid in the Body to subtract from. The equivalent warning appears if you accidentally invoke the subtractive variant on an empty Body.  
    **Fix:** Create at least one additive feature first (e.g. Additive Box), then add the Subtractive Box on top.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401 — registers PartDesign object types

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create an Additive Box inside the Body
box = doc.addObject("PartDesign::AdditiveBox", "Box")
body.addObject(box)

# Set dimensions
box.Length = 30.0  # mm
box.Width  = 20.0  # mm
box.Height = 10.0  # mm

doc.recompute()

print(f"Volume: {box.Shape.Volume:.4f} mm³")
# Expected: 30 × 20 × 10 = 6000.0000 mm³
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
| `BaseFeature` | `App::PropertyLink` | The feature this box is fused on top of. `None` if it is the first feature in the Body. |
| `AddSubShape` | `Part::PropertyPartShape` | The raw box shape before the boolean fuse operation. Useful for inspection. |

---

## See also

- [Subtractive Box](subtractive-box.md) — identical parameters, cuts material instead of adding it
- [Pad](pad.md) — extrude a sketch profile; more flexible for non-rectangular cross-sections
- [Additive Cylinder](additive-cylinder.md) — cylindrical primitive
- [Additive Sphere](additive-sphere.md) — spherical primitive
- [Additive Cone](additive-cone.md) — conical/frustum primitive
