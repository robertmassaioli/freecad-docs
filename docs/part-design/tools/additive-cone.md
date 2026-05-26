# Additive Cone

> **In one sentence:** Insert a parametric cone or frustum (truncated cone) directly into a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Cone  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveAdditive) · **Shortcut:** none

Additive Cone creates a solid cone or frustum defined by two radii (one per end face), a height, and an optional sweep angle, then fuses it into the active Body. No sketch is required. Setting the top radius to zero gives a true cone with a sharp apex; setting both radii to the same non-zero value gives a cylinder; any other combination gives a frustum (truncated cone). A sweep angle less than 360° cuts the shape into a sector.

---

## Intuition

Think of an ice-cream cone or a bucket. A cone narrows from a wide base to a point; a frustum (bucket shape) narrows from a wide base to a smaller flat top. The Additive Cone captures both shapes through two radius parameters:

- `Radius1` is the radius of the **bottom** face (at Z = 0).
- `Radius2` is the radius of the **top** face (at Z = Height).

When `Radius2 = 0`, the top collapses to a single point: a sharp apex. When `Radius2 > 0`, the top is a flat circular face: a frustum. Either radius can be larger than the other — you can have a cone that widens toward the top by making `Radius2 > Radius1`.

!!! note
    When `Radius1 == Radius2` (within floating-point precision), FreeCAD internally builds the shape as a cylinder rather than a degenerate cone. The resulting solid is identical in shape to [Additive Cylinder](additive-cylinder.md) with the same radius and height.

The **sweep angle** (`Angle`) works the same as in Additive Cylinder: at 360° you get the full rotational solid; at less than 360° you get a sector wedge.

---

## When to use it

- You need a conical boss, taper, or spigot without drawing a trapezoidal sketch to revolve.
- You need a frustum (truncated cone) for a tapered fitting, plug, or transition between two circular sizes.
- You want a sharp-pointed cone for a tip, spike, or reference geometry.
- You are scripting or generating parametric models and want a single API call to produce a cone feature.
- You need a conical sector (partial cone) by setting the sweep angle to less than 360°.

---

## When NOT to use it

- **The taper profile is non-linear (curved side wall)** — draw a curve profile as a sketch and use [Revolution](revolution.md). Additive Cone only produces straight-sided tapers.
- **You need a hollow cone or thin-walled conical shell** — a Pad of a ring sketch combined with a Shell operation is more appropriate.
- **You want to cut a conical pocket or tapered hole into existing material** — use [Subtractive Cone](subtractive-cone.md), which has identical parameters but cuts instead of adds.
- **The cone cross-section should be elliptical rather than circular** — use [Revolution](revolution.md) with an appropriate sketch, or [Additive Ellipsoid](additive-ellipsoid.md) if a partial ellipsoid is sufficient.

---

## Step-by-step walkthrough

**Prerequisite:** An active Part Design document. An active Body is required; FreeCAD will offer to create one if none exists.

1. **Invoke the tool.** Go to **Part Design → Additive Primitives → Additive Cone**, or click the Additive Cone icon in the CompPrimitiveAdditive toolbar drop-down.

2. **If no Body exists,** FreeCAD creates one automatically. If multiple Bodies exist, choose the target Body in the dialog.

3. **The task panel opens** with four fields: Radius1, Radius2, Height, and Angle.

4. **Set Radius1** — the base (bottom) radius. Default is 2 mm.

5. **Set Radius2** — the top radius. Default is 4 mm. Set to 0 for a sharp apex; set equal to Radius1 for a cylinder; set larger than Radius1 for an upward-flaring frustum.

6. **Set Height.** Default is 10 mm. Must be greater than zero.

7. **Set Angle.** Default is 360° (full rotational solid). Reduce to create a sector.

8. **Optionally configure Attachment** in the lower task panel section to position the cone relative to a face, edge, or datum.

9. **Click OK.** The cone is fused into the Body and appears in the model tree.

!!! tip
    To produce a standard sharp cone pointing upward, set Radius1 to your desired base radius, Radius2 to 0, and Height to the desired length. The apex will be at the top of the Z-axis.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Length (mm) | 2.0 mm | Radius of the bottom face (at Z = 0). Range: 0 mm to max float. May be 0 only if Radius2 > 0. |
| Radius2 | Length (mm) | 4.0 mm | Radius of the top face (at Z = Height). Range: 0 mm to max float. Set to 0 for a sharp apex. |
| Height | Length (mm) | 10.0 mm | Distance between the two circular faces along the local Z-axis. Must be greater than zero. |
| Angle | Angle (°) | 360.0° | Sweep angle of the cross-section around the central axis. Range: 0° (exclusive) to 360°. Values less than 360° produce a sector with two planar lateral faces. |

!!! note
    Both `Radius1` and `Radius2` may be zero, but not simultaneously — that would produce a degenerate shape with no volume. The source (`quantityRangeZero`) allows zero for both radii individually, but OCCT will reject a cone with both radii at zero.

The task panel also exposes an **Attachment** section (inherited from `Part::AttachExtension`) for positioning the cone relative to faces, edges, vertices, or datum objects.

---

## Advanced usage

### Inverted frustums

If Radius2 > Radius1, the cone flares outward toward the top — an inverted frustum. This is useful for draft-angle modelling, nozzle interiors, or funnel shapes.

### Zero top radius — sharp apex

Setting `Radius2 = 0.0` in the Python API produces a cone with a geometric point at the apex. Note that this creates a degenerate (non-manifold) vertex in the OCCT topology; downstream operations like Fillet on the apex edge may fail or behave unexpectedly. For most downstream purposes a very small `Radius2` (e.g. 0.01 mm) is safer than exactly zero.

### Using the cone as a construction aid

Cones placed with Additive Cone can be used as reference geometry for positioning other features. For example, place a cone at the centre of a circular boss to visualise draft angles, then delete or suppress the cone feature when it is no longer needed.

### Cone sector for angular geometry

Set `Angle` to 90° to create a quarter-cone sector. Combined with a polar pattern, this can tile into a full cone while retaining the sector as a discrete reusable feature.

---

## Common mistakes and pitfalls

!!! warning "Both Radius1 and Radius2 are zero — feature fails"
    **Cause:** If both radii are set to zero there is no volume to build. FreeCAD will report a geometry failure from OCCT.  
    **Fix:** Set at least one radius to a value greater than zero.

!!! warning "Expected a cone but got a cylinder"
    **Cause:** `Radius1` and `Radius2` are equal (or differ by less than OCCT's precision threshold). When the two radii are the same, FreeCAD internally uses `BRepPrimAPI_MakeCylinder` and produces a cylinder.  
    **Fix:** If you intended a frustum, adjust Radius1 and Radius2 to different values. This is intentional behaviour, not a bug.

!!! warning "Cone appears as a sector instead of a full solid"
    **Cause:** The `Angle` property is less than 360°.  
    **Fix:** Open the feature for editing and reset Angle to 360°.

!!! warning "Height too small — feature fails to compute"
    **Cause:** Height must be greater than approximately 2 × 10⁻⁷ mm. Setting it to zero triggers an error.  
    **Fix:** Use a positive Height value.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # noqa: F401 — registers PartDesign object types
import math

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create an Additive Cone (frustum: bottom radius 6, top radius 3, height 15)
cone = doc.addObject("PartDesign::AdditiveCone", "Cone")
body.addObject(cone)

cone.Radius1 = 6.0   # mm — bottom radius
cone.Radius2 = 3.0   # mm — top radius
cone.Height  = 15.0  # mm
cone.Angle   = 360.0 # full solid (degrees)

doc.recompute()

# Volume of a frustum: (π/3) × H × (R1² + R1·R2 + R2²)
expected = (math.pi / 3.0) * 15.0 * (6.0**2 + 6.0 * 3.0 + 3.0**2)
print(f"Volume: {cone.Shape.Volume:.4f} mm³")
# Expected: (π/3) × 15 × (36 + 18 + 9) ≈ 989.6018 mm³

# Sharp apex cone: Radius2 = 0
apex_cone = doc.addObject("PartDesign::AdditiveCone", "ApexCone")
body.addObject(apex_cone)
apex_cone.Radius1 = 5.0
apex_cone.Radius2 = 0.0
apex_cone.Height  = 12.0
apex_cone.Angle   = 360.0
doc.recompute()
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius1` | `App::PropertyLength` | `2.0 mm` | Bottom face radius. Range: [0, ∞). At least one radius must be > 0. |
| `Radius2` | `App::PropertyLength` | `4.0 mm` | Top face radius. Range: [0, ∞). Set to 0 for a sharp apex. |
| `Height` | `App::PropertyLength` | `10.0 mm` | Axial length. Must be > 0. |
| `Angle` | `App::PropertyAngle` | `360.0°` | Sweep angle around the central axis. Range: (0°, 360°]. |

---

## See also

- [Subtractive Cone](subtractive-cone.md) — identical parameters, cuts material instead of adding it
- [Additive Cylinder](additive-cylinder.md) — special case of the cone where Radius1 = Radius2
- [Additive Box](additive-box.md) — rectangular primitive
- [Additive Sphere](additive-sphere.md) — spherical primitive
- [Revolution](revolution.md) — revolve a sketch profile for curved-wall cones and non-circular sections
