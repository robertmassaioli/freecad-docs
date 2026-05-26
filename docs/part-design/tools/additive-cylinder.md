# Additive Cylinder

> **In one sentence:** Insert a parametric cylinder (or partial cylinder) directly into a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Cylinder  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveAdditive) · **Shortcut:** none

Additive Cylinder creates a solid cylinder defined by a radius, a height, and a sweep angle, and fuses it into the active Body. No sketch is required. An optional sweep angle less than 360° produces a partial ("pie-slice") cylinder. Two skew angles (FirstAngle and SecondAngle) can tilt the cylinder's axis in X and Y, turning the flat end caps into oblique cuts.

---

## Intuition

Picture a round pillar or a coin. The Additive Cylinder is exactly that: a disc of radius *R* extruded upwards by height *H*. The resulting solid is placed with its central axis along the Body's local Z-axis, its bottom face on the XY plane.

The **sweep angle** (property `Angle`) controls what fraction of the full circle is filled in. At the default of 360° you get a complete solid cylinder. At 180° you get a half-cylinder; at 90° a quarter-cylinder wedge. This is useful for sector-shaped lugs, cam profiles, or partial boss geometry.

The **skew angles** (`FirstAngle` and `SecondAngle`) tilt the extrusion direction. A non-zero FirstAngle leans the extrusion toward the local X-axis; SecondAngle leans it toward Y. The bottom face stays flat on the XY plane but the top face becomes a slanted cut. This is equivalent to extruding at an angle, without needing a sketch.

---

## When to use it

- You need a round pin, boss, pillar, or hub and want to skip drawing a circle sketch.
- You want a partial cylinder (sector shape) for a cam lug, sector gear segment, or pie-shaped relief.
- You need a skewed cylinder (angled top face) without going through the Sketcher.
- You are scripting models and want a single API call to produce a cylindrical feature.
- You are prototyping — placing a cylinder quickly to verify proportions before adding holes and blends.

---

## When NOT to use it

- **The cross-section is not circular** — draw the profile as a sketch and use [Pad](pad.md). Hexagonal bosses, oval protrusions, or any non-circular outline require a sketch.
- **The cylinder follows a curved axis** — use [Additive Pipe](additive-pipe.md) with a circular profile and a spine sketch.
- **You need to remove a cylindrical hole from existing material** — use [Subtractive Cylinder](subtractive-cylinder.md), which has identical parameters but cuts instead of adds. (For simple through-holes, [Hole](hole.md) is purpose-built and handles threads.) <!-- TODO: verify link to hole.md -->
- **The cylinder must be placed precisely relative to an existing circular face** — consider using the **Attachment** mode to snap the primitive, or create a sketch on that face and use Pad for more predictable behaviour.

---

## Step-by-step walkthrough

**Prerequisite:** An active Part Design document. An active Body is required; FreeCAD will offer to create one if none exists.

1. **Invoke the tool.** Go to **Part Design → Additive Primitives → Additive Cylinder**, or click the Additive Cylinder icon in the CompPrimitiveAdditive toolbar drop-down.

2. **If no Body exists,** FreeCAD creates one automatically. If multiple Bodies exist, choose the target Body in the dialog.

3. **The task panel opens** with four fields: Radius, Height, Angle, and the skew angles (labeled *X Skew* and *Y Skew* in the UI, corresponding to `FirstAngle` and `SecondAngle` in the Python API).

4. **Set Radius** — the circular cross-section radius in mm. Default is 10 mm.

5. **Set Height** — the axial length of the cylinder. Default is 10 mm.

6. **Set Angle** — the sweep angle of the cross-section. Default is 360° (full cylinder). Enter a smaller value for a partial cylinder.

7. **Optionally set skew angles.** FirstAngle tilts the extrusion toward X; SecondAngle toward Y. Both default to 0°. Range is −89.99999° to +89.99999°.

8. **Optionally configure Attachment** in the lower section of the task panel to position the cylinder relative to a face, edge, or datum.

9. **Click OK.** The cylinder is fused into the Body and appears in the model tree.

!!! tip
    For a simple round hole drilled into existing material, use [Subtractive Cylinder](subtractive-cylinder.md) rather than Additive Cylinder. For holes with standard thread specifications, use the dedicated [Hole](hole.md) tool instead. <!-- TODO: verify link to hole.md -->

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius | Length (mm) | 10.0 mm | Radius of the circular cross-section. Must be greater than zero. |
| Height | Length (mm) | 10.0 mm | Axial length (distance from bottom face to top face). Must be greater than zero. |
| Angle | Angle (°) | 360.0° | Sweep angle of the cross-section, measured around the central axis. Range: 0° (exclusive) to 360° (inclusive). Values less than 360° produce a partial cylinder with two flat planar faces cutting the arc. |
| First Angle | Angle (°) | 0.0° | Skew angle in the X direction. Tilts the extrusion vector toward the local X-axis, making the top face oblique. Range: −89.99999° to +89.99999°. |
| Second Angle | Angle (°) | 0.0° | Skew angle in the Y direction. Tilts the extrusion vector toward the local Y-axis. Range: −89.99999° to +89.99999°. |

The task panel also exposes an **Attachment** section (inherited from `Part::AttachExtension`) for positioning the cylinder relative to faces, edges, vertices, or datum objects.

---

## Advanced usage

### Partial cylinders for sector geometry

Setting `Angle` to values less than 360° is useful for cam profiles, sector gears, and angled protrusions. The two planar faces formed by the arc endpoints are perpendicular to the XY plane by default, and their angular positions are determined by the sweep angle starting at the local X-axis.

### Skewed cylinders

When `FirstAngle` or `SecondAngle` is non-zero, the top face of the cylinder becomes an oblique plane rather than a flat disc. This is equivalent to applying a shear to the extrusion direction. A cylinder with `FirstAngle = 30°` and a height of 10 mm will have its top face centre offset by `10 × tan(30°) ≈ 5.77 mm` in the X direction relative to the bottom face centre.

Both skew angles can be combined; the resulting extrusion direction is `(H·tan(α), H·tan(β), H)` in local coordinates, where α is FirstAngle and β is SecondAngle.

### Using expressions for parametric sizing

Right-click any numeric field in the task panel and choose **Set expression** to drive the dimension from a spreadsheet cell or master parameter. This is the standard approach for keeping multiple cylindrical features consistent across a parametric assembly.

---

## Common mistakes and pitfalls

!!! warning "Cylinder appears as a pie slice instead of a full cylinder"
    **Cause:** The `Angle` property was accidentally set to a value less than 360° — perhaps by mis-clicking the spinner.  
    **Fix:** Open the feature for editing (double-click in the model tree), find the Angle field, and set it back to 360°.

!!! warning "Radius or Height too small — feature fails to compute"
    **Cause:** Radius or Height has been set to zero or a negative value. FreeCAD enforces that both must be greater than approximately 2 × 10⁻⁷ mm.  
    **Fix:** Ensure Radius and Height are both strictly positive. Angle must also be strictly positive.

!!! warning "Top face is tilted when no skew was intended"
    **Cause:** A non-zero FirstAngle or SecondAngle was left over from a previous edit.  
    **Fix:** Reset both skew angles to 0° in the task panel.

!!! warning "Cylinder is in the wrong position relative to existing geometry"
    **Cause:** By default the cylinder is placed at the Body origin. If the Body origin is not where you expect the cylinder, it appears elsewhere.  
    **Fix:** Use the Attachment section in the task panel to snap the primitive to the desired face or datum, or add a Datum Plane and attach to it.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401 — registers PartDesign object types

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create an Additive Cylinder inside the Body
cyl = doc.addObject("PartDesign::AdditiveCylinder", "Cylinder")
body.addObject(cyl)

# Set dimensions
cyl.Radius = 8.0   # mm
cyl.Height = 25.0  # mm
cyl.Angle  = 360.0 # full cylinder (degrees)

doc.recompute()

import math
expected = math.pi * 8.0**2 * 25.0
print(f"Volume: {cyl.Shape.Volume:.4f} mm³")
# Expected: π × 8² × 25 ≈ 5026.5482 mm³
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | `App::PropertyLength` | `10.0 mm` | Circular cross-section radius. Must be > 0. |
| `Height` | `App::PropertyLength` | `10.0 mm` | Axial length. Must be > 0. |
| `Angle` | `App::PropertyAngle` | `360.0°` | Arc sweep angle. Range: (0°, 360°]. |
| `FirstAngle` | `App::PropertyAngle` | `0.0°` | Skew toward local X. Range: (−90°, 90°) exclusive. Provided by `Part::PrismExtension`. |
| `SecondAngle` | `App::PropertyAngle` | `0.0°` | Skew toward local Y. Range: (−90°, 90°) exclusive. Provided by `Part::PrismExtension`. |

---

## See also

- [Subtractive Cylinder](subtractive-cylinder.md) — identical parameters, cuts material instead of adding it
- [Additive Box](additive-box.md) — rectangular primitive
- [Additive Cone](additive-cone.md) — conical/frustum primitive; generalises the cylinder with two radii
- [Additive Sphere](additive-sphere.md) — spherical primitive
- [Pad](pad.md) — sketch-based extrusion; use when the cross-section is not circular
- [Additive Pipe](additive-pipe.md) — sweep a profile along a curved spine
