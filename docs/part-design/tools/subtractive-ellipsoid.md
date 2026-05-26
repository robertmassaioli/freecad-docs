# Subtractive Ellipsoid

> **In one sentence:** Cut an ellipsoid-shaped cavity — a stretched or squashed
> sphere — out of an existing Part Design Body solid.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Ellipsoid  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Subtractive Ellipsoid removes an ellipsoidal volume from the current tip of the
active Body using a Boolean cut. The ellipsoid is defined by up to three
independent radii (one per axis) and three angular clipping parameters that let
you cut partial ellipsoids — hemispheres, latitudinal bands, or swept wedge
sections. Because it requires no sketch, it is the fastest way to hollow out a
dome, lens, or egg-shaped pocket in a solid.

---

## Intuition

Think of pressing an egg-shaped die into clay: the die removes a cavity whose
shape is determined solely by the die's geometry — no path or profile sketch
needed. Subtractive Ellipsoid works the same way: you position a virtual
ellipsoid inside the existing solid, click OK, and FreeCAD subtracts it.

The ellipsoid's three radii let you independently stretch it along each local
axis. Radius1 scales the Z-axis (pole-to-pole height), Radius2 scales the X-axis
(equatorial width), and Radius3 scales the Y-axis. When Radius3 is left at `0`,
FreeCAD treats it as equal to Radius2, giving a body of revolution — convenient
for the common prolate (rugby ball) or oblate (discus) case.

The two latitude angles (Angle1 and Angle2) clip the bottom and top of the
ellipsoid like parallel cuts through a globe. The longitude angle (Angle3) sweeps
the ellipsoid around its Z-axis; reducing it below `360°` produces a pie-slice
cavity rather than a full ellipsoidal pocket.

The centre of the ellipsoid coincides with the Body's current placement origin
unless you reposition it with an Attachment.

---

## When to use it

- You need an egg-shaped, dome, or lens-shaped cavity in a solid and drawing and
  revolving a profile sketch would be slower.
- You want to thin out the end cap of a pressure vessel or tank model with a
  smooth curved pocket.
- You need a partial ellipsoidal cut — for example, a hemispherical socket or a
  shallow oblate recess — using the angle clipping parameters.
- You are iterating on a concept model and want to experiment with organic cavity
  shapes quickly by adjusting three radii.

---

## When NOT to use it

- **The required cavity is a perfect sphere** — use
  [Subtractive Sphere](subtractive-sphere.md) instead; it has a simpler single-radius
  parameter set.
- **You need to add material in an ellipsoidal shape** — use
  [Additive Ellipsoid](additive-ellipsoid.md), which is identical but fuses instead
  of cuts.
- **The ellipsoidal surface must follow a curved axis or path** — use a
  [Groove](groove.md) (revolve a sketch) for full control over the meridian profile.
- **No solid exists yet in the Body** — subtractive primitives require an existing
  base feature to cut from; FreeCAD will display an error and refuse to proceed.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body with at least one existing solid
feature (the Tip must be non-empty).

1. Ensure the Body you want to modify is active. If it is not, double-click it in
   the model tree.

2. Activate **Part Design → Subtractive Primitives → Subtractive Ellipsoid** from
   the menu, or click its icon in the Part Design Modeling Primitives toolbar. If
   multiple primitives share one toolbar button, expand the drop-down and select
   **Subtractive Ellipsoid**.

3. The task panel opens showing the six ellipsoid parameters. FreeCAD immediately
   displays a preview of the ellipsoid in the viewport.

4. Set **Radius1** (Z-axis) and **Radius2** (equatorial X) to the dimensions of
   the cavity you want. Leave **Radius3** at `0` for a rotationally symmetric
   ellipsoid.

5. Optionally adjust **Angle1** and **Angle2** to clip the latitude extent. The
   default `−90° / +90°` cuts the full ellipsoid from south pole to north pole.

6. Optionally reduce **Angle3** below `360°` to cut a swept wedge section rather
   than a full ellipsoidal cavity.

7. Click **OK**. FreeCAD performs the Boolean cut and the resulting Body shape is
   updated. The feature appears in the model tree as **Ellipsoid**.

!!! tip
    Use the Attachment tool (right-click the Subtractive Ellipsoid in the model
    tree → Attachment) to reposition the ellipsoid's local coordinate system onto
    any face, edge, or vertex before clicking OK — useful when the cavity must be
    centred on a specific face rather than the Body origin.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Length (mm) | 2.0 | Radius along the local Z-axis (polar/height direction). Must be > 0. |
| Radius2 | Length (mm) | 4.0 | Radius along the local X-axis (equatorial). Must be > 0. |
| Radius3 | Length (mm) | 0.0 | Radius along the local Y-axis. When `0`, FreeCAD uses the same value as Radius2, giving a rotationally symmetric ellipsoid. Set to a non-zero positive value for a fully triaxial ellipsoid. |
| Angle1 | Angle (°) | −90.0 | Lower latitude clipping angle. Range: −90° to +90°. Controls where the south-pole cap is cut. |
| Angle2 | Angle (°) | 90.0 | Upper latitude clipping angle. Range: −90° to +90°. Must be greater than Angle1. Controls where the north-pole cap is cut. |
| Angle3 | Angle (°) | 360.0 | Sweep angle around the Z-axis. Range: 0° to 360°. Values less than 360° produce a pie-slice wedge cavity. |

---

## Advanced usage

### Triaxial ellipsoidal cavities

Setting all three radii to different positive values produces a fully triaxial
cavity — no axis of rotational symmetry. This is useful for organic pockets such
as a saddle-shaped recess or an asymmetric socket. Radius3 = 0 is a shorthand
for Radius3 = Radius2; any non-zero positive value activates independent Y-axis
scaling.

### Combining latitude and longitude clipping

Setting both Angle1 and Angle2 to values within the same hemisphere (for example,
`Angle1 = 0°` and `Angle2 = 45°`) cuts only a shallow spherical cap, leaving the
bulk of the ellipsoid uncut. Combining this with `Angle3 < 360°` produces
crescent- or wedge-segment cavities that would be tedious to create with a
revolved sketch.

### Subtractive primitives vs. Pocket/Groove

Subtractive primitives are faster to set up than Pocket or Groove when the cavity
shape matches a standard geometric body exactly. For an ellipsoidal socket with
known radii, a Subtractive Ellipsoid is two clicks; the equivalent Groove would
require drawing and constraining a full meridian arc. Choose Pocket/Groove when
you need a profile that does not match any primitive, or when the feature must be
driven by sketch geometry already present in the model.

---

## Common mistakes and pitfalls

!!! warning "Cannot subtract primitive feature without base feature"
    **Cause:** The Body has no existing solid (the Tip is empty), so there is
    nothing to cut from.  
    **Fix:** Add at least one additive feature (such as a Pad or Additive Box) to
    the Body before applying a subtractive primitive.

!!! warning "Ellipsoid cavity does not appear / Boolean fails silently"
    **Cause:** The ellipsoid does not intersect the existing solid — it is
    entirely outside the Body's shape.  
    **Fix:** Check that the Body's origin (where the primitive is centred by
    default) lies inside the solid. Use the Attachment tool or adjust the Body
    placement so the ellipsoid overlaps the material to be removed.

!!! warning "Angle1 and Angle2 produce a very thin or empty cut"
    **Cause:** Angle1 and Angle2 are set too close together, leaving only a
    narrow latitudinal band.  
    **Fix:** Widen the range — set Angle1 to a negative value and Angle2 to a
    positive value that spans the intended portion.

!!! warning "Radius3 appears to have no effect"
    **Cause:** Radius3 is set to a positive value very close to Radius2, so the
    Y-axis deformation is imperceptible in the viewport.  
    **Fix:** Use a value clearly different from Radius2 (for example, half or
    double) to see the asymmetry. Also ensure the value was committed — press
    Enter after typing in the task panel field.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

# First create a base solid to cut from
box = doc.addObject("PartDesign::AdditiveBox", "Box")
body.addObject(box)
box.Length = 20.0
box.Width = 20.0
box.Height = 20.0
doc.recompute()

# Now subtract an ellipsoid from the centre of the box
ellipsoid = doc.addObject("PartDesign::SubtractiveEllipsoid", "Ellipsoid")
body.addObject(ellipsoid)

# Defaults: Radius1=2, Radius2=4, Radius3=0, Angle1=-90°, Angle2=90°, Angle3=360°
doc.recompute()

print(f"Volume: {ellipsoid.Shape.Volume:.4f} mm³")
# TODO: verify computed volume
```

### Customising radii and angles

```python
ellipsoid.Radius1 = 5.0    # Z-axis (polar)
ellipsoid.Radius2 = 8.0    # X-axis (equatorial)
ellipsoid.Radius3 = 6.0    # Y-axis (independent, triaxial)
ellipsoid.Angle1  = -45.0  # clip lower quarter
ellipsoid.Angle2  =  90.0  # full upper hemisphere
ellipsoid.Angle3  = 270.0  # three-quarter sweep around Z
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Radius1` | `App::PropertyLength` | Radius along local Z (polar direction), mm. |
| `Radius2` | `App::PropertyLength` | Radius along local X (equatorial), mm. |
| `Radius3` | `App::PropertyLength` | Radius along local Y; `0` means equal to Radius2. |
| `Angle1` | `App::PropertyAngle` | Lower latitude clip angle, degrees. Range −90° to +90°. |
| `Angle2` | `App::PropertyAngle` | Upper latitude clip angle, degrees. Range −90° to +90°. |
| `Angle3` | `App::PropertyAngle` | Longitudinal sweep around Z-axis, degrees. Range 0° to 360°. |

---

## See also

- [Additive Ellipsoid](additive-ellipsoid.md) — same parameters, adds material instead of removing it
- [Subtractive Sphere](subtractive-sphere.md) — simpler single-radius spherical cavity
- [Subtractive Torus](subtractive-torus.md) — donut-shaped cavity with similar angle parameters
- [Groove](groove.md) — revolve a custom sketch profile for non-standard curved cavities
- [Pocket](pocket.md) — extrude a sketch downward to remove material
