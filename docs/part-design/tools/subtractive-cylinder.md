# Subtractive Cylinder

> **In one sentence:** Drill a cylindrical hole or cut a cylindrical recess into an existing solid in a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Cylinder  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveSubtractive) · **Shortcut:** none

Subtractive Cylinder creates a parametric cylinder — defined by a radius, height, and optional sweep angle — and subtracts it from the active Body's current solid using a boolean cut. It is the subtractive counterpart of [Additive Cylinder](additive-cylinder.md) and shares identical parameters. No sketch is required: the cylinder is placed and sized immediately through a task panel.

---

## Intuition

Think of pressing a round drill bit straight through a block of material. The drill (the cylinder) removes everything inside its circular cross-section for the full height you specify. When you pull the drill out, a perfectly round hole remains.

The Angle parameter adds a twist: instead of a full 360° cylinder, you can create a partial cylinder — a slice, like a quarter-pie cut or a half-drum channel. At 360° you get a complete cylinder; at 180° you get a half-cylinder groove.

The cylinder's axis runs along the Body's local Z-axis by default. The **Attachment** section lets you reorient the cut to any face or datum plane, so you can drill from any direction without repositioning the model.

---

## When to use it

- You need a circular through-hole or blind hole whose diameter is parametrically defined, with no sketch required.
- You want to cut a cylindrical boss socket, bearing seat, or shaft hole directly and quickly.
- You are doing rapid concept modelling and want to remove cylindrical material before committing to sketch-based features.
- You need a partial cylindrical channel (Angle < 360°) — for example, a half-round slot or a sector-shaped cutout.
- You are scripting models and want a single API call to produce a cylindrical cut.

---

## When NOT to use it

- **The hole profile is not a simple circle** — draw the profile as a sketch and use [Pocket](pocket.md) instead. Any non-circular cross-section (hex hole, slotted hole, countersink) requires a sketch.
- **You need a countersunk or stepped hole** — use [Pocket](pocket.md) or the [Hole](hole.md) tool, which has built-in support for standard fastener geometries.
- **You need the hole to follow a pattern** — use [Pocket](pocket.md) with a sketch, which integrates more naturally with Part Design pattern tools (Polar Pattern, Linear Pattern).
- **You want to add cylindrical material** — use [Additive Cylinder](additive-cylinder.md), which has identical parameters but fuses instead of cuts.
- **No solid exists in the Body yet** — a subtractive feature requires an existing base feature to subtract from.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one existing additive feature (the solid to cut into).

1. **Activate Part Design.** Open or switch to the Part Design workbench.

2. **Invoke the tool.** Go to **Part Design → Subtractive Primitives → Subtractive Cylinder**. Alternatively, click the Subtractive Cylinder icon in the toolbar (inside the CompPrimitiveSubtractive drop-down button).

3. **The task panel opens** showing Radius, Height, and Angle fields. Defaults are Radius = 10 mm, Height = 10 mm, Angle = 360°. The 3-D view shows the cylinder as a preview shape.

4. **Set the dimensions.** Enter the Radius and Height of the cylindrical cut. Leave Angle at 360° for a full cylinder (typical hole). Reduce it for a partial sector cut.

5. **Optionally set an attachment.** Use the **Attachment** section to align the cylinder axis to an existing face, edge, or datum. This controls the direction of drilling.

6. **Click OK.** The cylinder is subtracted from the Body.

!!! tip
    To drill from the side rather than the top, attach the cylinder to a side face of the solid or to a datum plane aligned with the desired drilling direction. The cylinder axis always runs along the local Z of its attachment frame.

!!! warning "No base feature available"
    If the Body has no tip feature, FreeCAD shows a warning and cancels. Add at least one additive feature first.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius | Length (mm) | 10.0 mm | Radius of the cylinder cross-section. Must be greater than zero. |
| Height | Length (mm) | 10.0 mm | Length of the cylinder along its axis (depth of the cut). Must be greater than zero. |
| Angle | Angle (°) | 360.0° | Sweep angle of the cylinder around its axis. Range: 0° < Angle ≤ 360°. Values below 360° produce a partial cylindrical sector rather than a complete cylinder. |

The task panel also exposes the **Attachment** section (inherited from `Part::AttachExtension`), which controls the position and orientation of the cylinder's axis. See the [Datum Plane](datum-plane.md) documentation for the full list of modes. <!-- TODO: verify link -->

---

## Advanced usage

### Partial cylindrical cuts

Setting Angle to a value less than 360° produces a wedge-shaped sector cut rather than a full cylinder. For example, an Angle of 180° cuts a half-drum groove — useful for split-housing designs or routing channels. The sector starts at the local X-axis and sweeps counter-clockwise when viewed from the positive Z direction.

### Driving dimensions with expressions

Radius, Height, and Angle all accept FreeCAD expressions. This makes it straightforward to tie the hole diameter to a named parameter (e.g. `Params.BoltDiameter / 2`) in a master spreadsheet.

### Stacking multiple cylindrical cuts

For counterbore or countersink approximations without the Hole tool, stack two Subtractive Cylinders with different radii and heights, both attached to the same face. The larger-radius, shallower cylinder creates the counterbore; the smaller-radius, deeper cylinder creates the clearance hole.

---

## Common mistakes and pitfalls

!!! warning "Hole appears on the wrong axis"
    **Cause:** The cylinder axis defaults to the Body's local Z-axis, which may not be the direction you want to drill.  
    **Fix:** Use the **Attachment** section to attach the cylinder to the face you want to drill into, or to a datum plane aligned with the desired direction.

!!! warning "Cannot create the cylinder — radius or height too small"
    **Cause:** FreeCAD enforces a minimum positive value for Radius and Height. Setting either to zero or near-zero triggers an error (`Radius of cylinder too small` or `Height of cylinder too small`).  
    **Fix:** Ensure Radius and Height are both greater than zero.

!!! warning "Rotation angle too small"
    **Cause:** Setting Angle to zero or a very small value (below `Precision::Confusion()`) triggers an error (`Rotation angle of cylinder too small`).  
    **Fix:** Keep Angle above zero. For a full cylinder, use 360°.

!!! warning "Resulting shape is not a solid"
    **Cause:** The boolean cut produced a degenerate result — for example, the cylinder is positioned entirely outside the base solid.  
    **Fix:** Check the attachment and position so the cylinder overlaps the existing solid.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401 — registers PartDesign object types

doc = App.newDocument()

# Create a Body with a base solid to cut into
body = doc.addObject("PartDesign::Body", "Body")
base = doc.addObject("PartDesign::AdditiveBox", "Base")
body.addObject(base)
base.Length = 50.0
base.Width  = 50.0
base.Height = 20.0
doc.recompute()

# Create a Subtractive Cylinder — a through-hole of radius 8 mm
hole = doc.addObject("PartDesign::SubtractiveCylinder", "Hole")
body.addObject(hole)

hole.Radius = 8.0   # mm
hole.Height = 20.0  # mm — full depth of the base solid
hole.Angle  = 360.0 # degrees — full circle

doc.recompute()

import math
expected_vol = 50 * 50 * 20 - math.pi * 8**2 * 20
print(f"Volume after cut: {hole.Shape.Volume:.4f} mm³")
# Expected: 50000 − π × 64 × 20 ≈ 45973.0 mm³
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | `App::PropertyLength` | `10.0 mm` | Radius of the cylinder. Must be > 0. |
| `Height` | `App::PropertyLength` | `10.0 mm` | Height (depth) of the cylinder along its axis. Must be > 0. |
| `Angle` | `App::PropertyAngle` | `360.0°` | Sweep angle around the cylinder axis. Range: (0°, 360°]. |

Properties inherited from `PartDesign::FeatureAddSub`:

| Property | Type | Description |
|----------|------|-------------|
| `BaseFeature` | `App::PropertyLink` | The feature this cut is applied to. |
| `AddSubShape` | `Part::PropertyPartShape` | The raw cylinder shape before the boolean cut. |

---

## See also

- [Additive Cylinder](additive-cylinder.md) — identical parameters, adds cylindrical material instead of removing it
- [Pocket](pocket.md) — sketch-based cut; supports non-circular profiles and integrates with pattern tools
- [Hole](hole.md) — specialised tool for standard fastener holes (countersink, counterbore, thread) <!-- TODO: verify page exists -->
- [Subtractive Box](subtractive-box.md) — rectangular cut primitive
- [Subtractive Sphere](subtractive-sphere.md) — spherical cut primitive
- [Subtractive Cone](subtractive-cone.md) — conical/frustum cut primitive
