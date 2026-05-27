# Subtractive Cone

> **In one sentence:** Cut a conical or frustum-shaped cavity out of an existing solid in a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Cone  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveSubtractive) · **Shortcut:** none

Subtractive Cone creates a parametric cone or truncated cone (frustum) — defined by two radii, a height, and an optional sweep angle — and subtracts it from the active Body's current solid using a boolean cut. It is the subtractive counterpart of [Additive Cone](additive-cone.md) and shares identical parameters. No sketch is required: the cone is placed and sized immediately through a task panel.

---

## Intuition

Picture a metalworker pressing a conical punch down into a blank. The punch removes a tapered void: wide at the top, narrow at the bottom (or vice versa depending on orientation). The two radii let you control both ends of the taper independently.

When Radius1 (the bottom) is zero, the result is a true cone — a sharp point at one end. When both radii are equal, the geometry degenerates into a cylinder (FreeCAD handles this automatically). When both radii are non-zero and different, you get a frustum — a truncated cone — which is the most common form used for chamfered recesses, tapered shaft seats, and funnel-shaped cavities.

The **Angle** parameter trims the cone around its axis: at 360° you get a complete cone or frustum; at 180° you get a half-cone trough; at 90° you get a quarter-cone wedge.

---

## When to use it

- You need a tapered hole, countersink, or conical recess whose taper dimensions are defined parametrically.
- You are cutting a frustum-shaped seat for a tapered pin, shaft, or insert.
- You want a chamfered entry on a hole without the full [Hole](hole.md) tool (approximate the countersink as a Subtractive Cone stacked over a Subtractive Cylinder).
- You need a funnel-shaped or V-groove cavity in a solid.
- You are scripting models and want a single API call to produce a conical cut.

---

## When NOT to use it

- **You need a standard countersink with a precise angle** — use the [Hole](hole.md) tool, which has built-in countersink and counterbore geometry matched to fastener standards. <!-- TODO: verify page exists -->
- **The cavity cross-section is not circular or tapered** — draw the profile as a sketch and use [Pocket](pocket.md) instead.
- **You need the cut to follow a pattern** — use [Pocket](pocket.md) with a revolved sketch, which integrates better with Part Design pattern tools.
- **You want to add conical material** — use [Additive Cone](additive-cone.md), which has identical parameters but fuses instead of cuts.
- **No solid exists in the Body yet** — a subtractive feature requires an existing base feature to subtract from.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one existing additive feature (the solid to cut into).

1. **Activate Part Design.** Open or switch to the Part Design workbench.

2. **Invoke the tool.** Go to **Part Design → Subtractive Primitives → Subtractive Cone**. Alternatively, click the Subtractive Cone icon in the toolbar (inside the CompPrimitiveSubtractive drop-down button).

3. **The task panel opens** showing Radius1, Radius2, Height, and Angle fields. Defaults are Radius1 = 2 mm, Radius2 = 4 mm, Height = 10 mm, Angle = 360°. The 3-D view shows the cone/frustum as a preview shape.

4. **Set the dimensions.** Enter Radius1 (bottom face radius), Radius2 (top face radius), and Height. Either radius can be set to 0 for a true cone that terminates in a point. Set Angle to less than 360° only if you want a partial sector cut.

5. **Optionally set an attachment.** Use the **Attachment** section to align the cone axis to an existing face, edge, or datum.

6. **Click OK.** The cone/frustum is subtracted from the Body.

!!! tip
    The cone's axis runs along local Z, with Radius1 at the base (Z = 0) and Radius2 at the top (Z = Height). To orient the wide end toward a particular face, set that face as the attachment and adjust Radius1/Radius2 accordingly, or flip the Z direction with an Attachment offset rotation of 180°.

!!! warning "No base feature available"
    If the Body has no tip feature, FreeCAD shows a warning and cancels. Add at least one additive feature first.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Length (mm) | 2.0 mm | Radius of the cone's bottom face (at Z = 0). Must be ≥ 0. Can be 0 to produce a true cone with a point at the base. |
| Radius2 | Length (mm) | 4.0 mm | Radius of the cone's top face (at Z = Height). Must be ≥ 0. Can be 0 to produce a true cone with a point at the top. |
| Height | Length (mm) | 10.0 mm | Height of the cone along its axis. Must be greater than zero. |
| Angle | Angle (°) | 360.0° | Sweep angle around the axis. Range: 0° < Angle ≤ 360°. Values below 360° produce a partial conical sector cut. |

!!! note "Equal radii"
    When Radius1 equals Radius2 (within floating-point precision), FreeCAD automatically builds a cylinder instead of a cone. This is handled transparently in the source — no error is raised.

The task panel also exposes the **Attachment** section (inherited from `Part::AttachExtension`), which controls the position and orientation of the cone's axis. See the [Datum Plane](datum-plane.md) documentation for the full list of modes. <!-- TODO: verify link -->

---

## Advanced usage

### Approximating a countersink

Stack a Subtractive Cone over a Subtractive Cylinder to approximate a countersunk hole:

1. Create a Subtractive Cylinder with the clearance-hole radius and full depth, attached to the entry face.
2. Create a Subtractive Cone with Radius1 = clearance radius, Radius2 = countersink outer radius, Height = countersink depth, also attached to the same face.

This gives a reasonable visual approximation. For production drawings requiring exact fastener-standard geometry, use the [Hole](hole.md) tool instead. <!-- TODO: verify page exists -->

### V-groove cuts

For a V-groove channel, use Radius1 = 0 (sharp tip) and set Radius2 to the groove half-width at the surface. Place the cone so its tip points into the material. Alternatively, flip the orientation so Radius1 is at the surface and the tip extends into the solid.

### Driving dimensions with expressions

All four parameters (Radius1, Radius2, Height, Angle) accept FreeCAD expressions. Bind the taper dimensions to a master spreadsheet to produce fully parametric fastener geometry that updates when the standard changes.

### Partial sector cuts

Setting Angle below 360° produces a wedge-shaped sector cavity. An Angle of 90° with Radius1 = 0 gives a quarter-cone notch — useful for corner relief cuts or indexing features.

---

## Common mistakes and pitfalls

!!! warning "Cone appears on the wrong axis or in the wrong location"
    **Cause:** The cone axis defaults to the Body's local Z-axis. If the origin is not near the face you want to cut, the subtraction happens in an unexpected location.  
    **Fix:** Use the **Attachment** section to attach the cone to the desired face or datum plane.

!!! warning "Cannot create the cone — height too small"
    **Cause:** FreeCAD enforces a minimum positive height. Setting Height to zero or near-zero triggers an error (`Height of cone too small`).  
    **Fix:** Ensure Height is greater than zero.

!!! warning "Cannot create the cone — negative radius"
    **Cause:** Radius1 or Radius2 cannot be negative. Setting either below zero triggers an error (`Radius of cone cannot be negative`).  
    **Fix:** Ensure both radii are zero or positive. Use zero for a true cone tip; use equal positive values for a cylinder.

!!! warning "Resulting shape is not a solid"
    **Cause:** The cone is positioned entirely outside the base solid, or both radii are zero (degenerate shape).  
    **Fix:** Ensure at least one radius is greater than zero and that the cone overlaps the existing solid.

!!! warning "Unexpected cylindrical result instead of conical"
    **Cause:** Radius1 equals Radius2 (intentionally or by mistake). FreeCAD silently builds a cylinder in this case.  
    **Fix:** Set Radius1 and Radius2 to different values to obtain a frustum. If a cylinder is what you need, use [Subtractive Cylinder](subtractive-cylinder.md) instead for clarity.

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
base.Length = 40.0
base.Width  = 40.0
base.Height = 30.0
doc.recompute()

# Create a Subtractive Cone — a frustum cavity
# Bottom radius 5 mm, top radius 10 mm, height 15 mm (full cone, 360°)
recess = doc.addObject("PartDesign::SubtractiveCone", "FrustumRecess")
body.addObject(recess)

recess.Radius1 = 5.0   # mm — bottom (Z=0) face radius
recess.Radius2 = 10.0  # mm — top (Z=Height) face radius
recess.Height  = 15.0  # mm
recess.Angle   = 360.0 # degrees — full revolution

doc.recompute()

import math
frustum_vol = (math.pi * 15 / 3) * (5**2 + 5*10 + 10**2)
print(f"Volume after cut: {recess.Shape.Volume:.4f} mm³")
# Base vol: 40×40×30 = 48000; frustum vol ≈ π×5×175/1 ≈ 2748.9 mm³
# Expected result ≈ 48000 − 2748.9 ≈ 45251.1 mm³
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius1` | `App::PropertyLength` | `2.0 mm` | Radius at the bottom face (Z = 0). Must be ≥ 0. |
| `Radius2` | `App::PropertyLength` | `4.0 mm` | Radius at the top face (Z = Height). Must be ≥ 0. |
| `Height` | `App::PropertyLength` | `10.0 mm` | Height of the cone along its axis. Must be > 0. |
| `Angle` | `App::PropertyAngle` | `360.0°` | Sweep angle around the axis. Range: (0°, 360°]. |

Properties inherited from `PartDesign::FeatureAddSub`:

| Property | Type | Description |
|----------|------|-------------|
| `BaseFeature` | `App::PropertyLink` | The feature this cut is applied to. |
| `AddSubShape` | `Part::PropertyPartShape` | The raw cone/frustum shape before the boolean cut. |

---

## See also

- [Additive Cone](additive-cone.md) — identical parameters, adds conical material instead of removing it
- [Pocket](pocket.md) — sketch-based cut with a revolved profile; more flexible for non-conical tapered cuts
- [Hole](hole.md) — specialised tool for standard countersink and counterbore fastener holes <!-- TODO: verify page exists -->
- [Subtractive Box](subtractive-box.md) — rectangular cut primitive
- [Subtractive Cylinder](subtractive-cylinder.md) — cylindrical cut primitive
- [Subtractive Sphere](subtractive-sphere.md) — spherical cut primitive
