# Additive Sphere

> **In one sentence:** Insert a parametric sphere (or spherical segment) directly into a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Sphere  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveAdditive) · **Shortcut:** none

Additive Sphere creates a solid sphere defined by a radius and three optional cutting angles, then fuses it into the active Body. No sketch is required. The three angles allow you to create latitudinal slices (Angle1 and Angle2, clipping the top and bottom) and a longitudinal wedge (Angle3, trimming the arc around the vertical axis), producing anything from a full sphere to a small spherical cap or wedge.

---

## Intuition

Think of a ball of clay dropped into your model. The full sphere is a perfect round ball with its centre at the Body's origin. The three cutting angles work like three separate knives:

- **Angle1** and **Angle2** are latitude cuts. Angle1 clips the bottom of the sphere (default −90°, at the south pole), and Angle2 clips the top (default +90°, at the north pole). Pull Angle1 up from −90° toward 0° and you shave off the bottom hemisphere, leaving a dome. Pull Angle2 down from +90° and you shave the top.
- **Angle3** is a longitude cut. At the default of 360°, the sphere wraps completely around its vertical axis. Reducing it produces a longitudinal wedge — a sphere sector shaped like a slice of orange.

These three angles can be combined to produce spherical caps, segments, zones, and wedges for tasks like decorative knobs, ball joints, dome features, and optical lens profiles.

---

## When to use it

- You need a spherical boss, knob, dome, or ball end without drawing a half-circle sketch to revolve.
- You want a spherical cap or dome (partial sphere) by adjusting the latitude angles.
- You are building a ball-and-socket joint or similar mechanism component and want a spherical surface to start from.
- You need to place a sphere at a specific location relative to an existing face using the Attachment system.
- You are scripting models and want a single API call to produce a spherical feature.

---

## When NOT to use it

- **The cross-section is not spherical** — use [Revolution](revolution.md) with a sketch profile for ellipsoids, paraboloids, or other surfaces of revolution. (For a true ellipsoid, see [Additive Ellipsoid](additive-ellipsoid.md).)
- **You need to remove a spherical depression from existing material** — use [Subtractive Sphere](subtractive-sphere.md), which has identical parameters but cuts instead of adds.
- **You need fine control over the surface normal at the equator** — in that case a revolve sketch with a controlled profile gives more precision.
- **The sphere needs to blend smoothly into adjacent geometry via a fillet** — the Part Design [Fillet](../tools/fillet.md) tool operates on the edges of the resulting Body; plan the edge topology before choosing a primitive. <!-- TODO: verify link -->

---

## Step-by-step walkthrough

**Prerequisite:** An active Part Design document. An active Body is required; FreeCAD will offer to create one if none exists.

1. **Invoke the tool.** Go to **Part Design → Additive Primitives → Additive Sphere**, or click the Additive Sphere icon in the CompPrimitiveAdditive toolbar drop-down.

2. **If no Body exists,** FreeCAD creates one automatically. If multiple Bodies exist, choose the target Body in the dialog.

3. **The task panel opens** with four fields: Radius, Angle1, Angle2, and Angle3.

4. **Set Radius.** Default is 5 mm.

5. **Set Angle1 (latitude start).** This is the lower latitude cut. Default is −90° (full hemisphere below the equator is included). Increasing this toward 0° or higher clips the bottom of the sphere.

6. **Set Angle2 (latitude end).** Default is +90° (full hemisphere above the equator included). Decreasing this toward 0° clips the top.

    !!! warning
        Angle1 must always be less than or equal to Angle2. The UI enforces this interactively: changing Angle1 raises the minimum of Angle2, and vice versa.

7. **Set Angle3 (longitude sweep).** Default is 360° (full revolution). Reducing this value produces a longitudinal wedge.

8. **Optionally configure Attachment** in the lower section of the task panel.

9. **Click OK.** The sphere is fused into the Body and appears in the model tree.

!!! tip
    For a hemisphere, set Angle1 = 0° and Angle2 = 90° (or Angle1 = −90° and Angle2 = 0° for the lower half). For a spherical cap, push Angle1 to a positive value close to Angle2.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius | Length (mm) | 5.0 mm | Radius of the sphere. Must be greater than zero. |
| Angle1 | Angle (°) | −90.0° | Lower latitude cut. Range: −90° to +90°. Must be less than or equal to Angle2. At −90° the full lower hemisphere is included. |
| Angle2 | Angle (°) | +90.0° | Upper latitude cut. Range: −90° to +90°. Must be greater than or equal to Angle1. At +90° the full upper hemisphere is included. |
| Angle3 | Angle (°) | 360.0° | Longitudinal sweep angle around the vertical axis. Range: 0° (exclusive) to 360°. Values less than 360° produce a wedge-shaped sector. |

The task panel also exposes an **Attachment** section (inherited from `Part::AttachExtension`) for positioning the sphere relative to faces, edges, vertices, or datum objects.

---

## Advanced usage

### Spherical caps and domes

To produce an upward-facing dome of radius *R* sitting on a flat circular base:

- Set Angle1 = 0° (the equatorial plane becomes the flat face)
- Set Angle2 = 90° (the north pole is the apex)
- The base circle will have radius *R* and the dome height will also be *R*

For a shallower cap that subtends only the top 30° of latitude, set Angle1 = 60° and Angle2 = 90°.

### Spherical wedges for sector geometry

Combining a full latitude range (Angle1 = −90°, Angle2 = 90°) with a reduced Angle3 produces a spherical wedge — useful for dividing a sphere into equal sectors or for constructing sector-shaped grips and handles.

### Positioning the sphere centre

By default the sphere centre is at the Body origin. Use the **Attachment** section to attach the sphere to an existing vertex, edge midpoint, or datum point, moving the centre to that location. Attachment Offset then lets you shift it further.

---

## Common mistakes and pitfalls

!!! warning "Angle1 greater than Angle2 — feature fails or produces no solid"
    **Cause:** Angle1 must always be less than Angle2. If they are equal the sphere collapses to a disc with zero volume, which OCCT cannot build into a valid solid.  
    **Fix:** Ensure Angle1 < Angle2. The UI enforces this interactively, but the Python API does not prevent you from setting conflicting values.

!!! warning "Sphere appears very small — radius is in wrong units"
    **Cause:** FreeCAD's internal unit is mm. If you set `cyl.Radius = 5` via Python you get 5 mm, not 5 cm.  
    **Fix:** Always specify dimensions in mm when using the Python API, or use `App.Units.Quantity` with explicit units.

!!! warning "Expected a full sphere but got a wedge"
    **Cause:** Angle3 was accidentally set to a value less than 360°.  
    **Fix:** Reset Angle3 to 360° in the task panel or via `sphere.Angle3 = 360.0`.

!!! warning "Sphere centre is at the origin instead of on the intended face"
    **Cause:** The Attachment section was not configured; the default placement puts the sphere centre at the Body origin.  
    **Fix:** Use the Attachment section to attach the sphere to a face or datum, or apply a Placement offset to move it.

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

# Create a full Additive Sphere
sphere = doc.addObject("PartDesign::AdditiveSphere", "Sphere")
body.addObject(sphere)

sphere.Radius = 10.0   # mm
sphere.Angle1 = -90.0  # full lower hemisphere
sphere.Angle2 =  90.0  # full upper hemisphere
sphere.Angle3 = 360.0  # full revolution

doc.recompute()

expected = (4.0 / 3.0) * math.pi * 10.0**3
print(f"Volume: {sphere.Shape.Volume:.4f} mm³")
# Expected: (4/3)π × 10³ ≈ 4188.7902 mm³
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | `App::PropertyLength` | `5.0 mm` | Sphere radius. Must be > 0. |
| `Angle1` | `App::PropertyAngle` | `−90.0°` | Lower latitude cut. Range: [−90°, +90°]. Must be ≤ Angle2. |
| `Angle2` | `App::PropertyAngle` | `+90.0°` | Upper latitude cut. Range: [−90°, +90°]. Must be ≥ Angle1. |
| `Angle3` | `App::PropertyAngle` | `360.0°` | Longitudinal sweep angle. Range: (0°, 360°]. |

---

## See also

- [Subtractive Sphere](subtractive-sphere.md) — identical parameters, cuts material instead of adding it
- [Additive Box](additive-box.md) — rectangular primitive
- [Additive Cylinder](additive-cylinder.md) — cylindrical primitive
- [Additive Cone](additive-cone.md) — conical/frustum primitive
- [Revolution](revolution.md) — revolve a sketch profile for non-spherical surfaces of revolution
- [Additive Ellipsoid](additive-ellipsoid.md) — ellipsoidal primitive with independent radii per axis
