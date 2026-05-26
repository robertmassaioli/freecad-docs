# Additive Ellipsoid

> **In one sentence:** Add a solid ellipsoid — a stretched or squashed sphere —
> directly into a Part Design Body, fusing it with any existing material.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Ellipsoid  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Additive Ellipsoid inserts a parametric ellipsoid solid into the active Body and
fuses it with the existing shape. The ellipsoid is defined by up to three
independent radii (one for each axis) and three angular clipping parameters that
let you create partial ellipsoids — hemispheres, wedge-cut sections, or truncated
caps. Because it is a primitive with no sketch dependency, it is the fastest way
to add an egg, lens, or dome shape to a model.

---

## Intuition

An ellipsoid is what you get when you take a sphere and independently scale it
along each of its three axes. If all three radii are equal you have a perfect
sphere; if two are equal and one differs you have a prolate (rugby-ball) or oblate
(discus) ellipsoid; with all three different you get a fully general triaxial
ellipsoid.

Two pairs of angle parameters trim the solid in different ways. Angle1 and Angle2
work like latitude lines on a globe: they clip the top and bottom of the ellipsoid
by rotating a meridian arc from the south pole to the north pole. Setting
`Angle1 = -90°` and `Angle2 = 90°` gives the full height; pulling them inward
produces a band or truncated cap. Angle3 works like a longitude sweep: it controls
how far around the Z-axis the ellipsoid extends, so `360°` is a full solid of
revolution, while smaller values produce a pie-slice wedge.

The local coordinate system places the centre of the ellipsoid at the Body's
origin. Radius1 scales along the local Z-axis (height), Radius2 along the local
X-axis (equatorial X), and Radius3 along the local Y-axis. When Radius3 is set
to `0`, FreeCAD treats it as equal to Radius2, giving a symmetric shape around the
Z-axis — convenient for the common prolate/oblate case.

---

## When to use it

- You need an egg-shaped boss, dome, or lens on a model and a sphere would be too
  round or a cone too sharp.
- You want to add an oblate cap (like the end of a pressure vessel) without
  drawing and revolving a profile.
- You are building a concept model and need a rough organic shape that you can
  refine later by adjusting three radii.
- You need a partial ellipsoid — for example, a hemisphere or a quarter section
  — using the angle clipping parameters.

---

## When NOT to use it

- **The required shape is a perfect sphere** — use
  [Additive Sphere](additive-sphere.md) instead; it has a simpler parameter set.
- **The ellipsoidal surface is just one face of a more complex body** — use
  [Additive Loft](additive-loft.md) or a revolved sketch for full control over the
  profile.
- **You want to remove material in an ellipsoidal shape** — use
  [Subtractive Ellipsoid](subtractive-ellipsoid.md), which has identical parameters
  but cuts instead of adds.
- **You need a shell or hollow ellipsoid** — primitives always produce solids;
  use a [Pocket](pocket.md) or [Thickness](thickness.md) operation after placing
  the ellipsoid.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body in the document. The ellipsoid can be
the very first feature (no existing solid required).

1. Open or create a document and ensure a Body is active. If no Body exists,
   FreeCAD will offer to create one automatically when you invoke the command.

2. Activate **Part Design → Additive Primitives → Additive Ellipsoid** from the
   menu, or click its icon in the Part Design Modeling Primitives toolbar. If
   multiple primitives share a single toolbar button, expand the drop-down and
   select **Additive Ellipsoid**.

3. The task panel opens. Set **Radius1** (Z-axis height radius) and **Radius2**
   (equatorial X radius) to the sizes you need. Leave **Radius3** at `0` if you
   want a rotationally symmetric ellipsoid.

4. Optionally adjust **Angle1** and **Angle2** to clip the bottom and top of the
   ellipsoid. The default `−90° / +90°` gives the full ellipsoid from pole to pole.

5. Optionally reduce **Angle3** below `360°` to create a partial swept section
   (pie-wedge ellipsoid).

6. Click **OK**. The ellipsoid is fused into the Body and appears in the model
   tree as **Ellipsoid**.

!!! tip
    To create a perfect hemisphere, set `Angle1 = 0°` and `Angle2 = 90°` (upper
    half) or `Angle1 = −90°` and `Angle2 = 0°` (lower half). Ensure Radius1 equals
    Radius2 for a true hemisphere rather than a semi-ellipsoid.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Length (mm) | 2.0 | Radius along the local Z-axis (polar/height direction). Must be > 0. |
| Radius2 | Length (mm) | 4.0 | Radius along the local X-axis (equatorial). Must be > 0. |
| Radius3 | Length (mm) | 0.0 | Radius along the local Y-axis. When `0`, FreeCAD uses the same value as Radius2, giving a rotationally symmetric ellipsoid. Set to a non-zero value for a fully triaxial ellipsoid. |
| Angle1 | Angle (°) | −90.0 | Lower latitude clipping angle. Range: −90° to +90°. Combined with Angle2 this controls how much of the polar arc is included. |
| Angle2 | Angle (°) | 90.0 | Upper latitude clipping angle. Range: −90° to +90°. Must be greater than Angle1. |
| Angle3 | Angle (°) | 360.0 | Sweep angle around the Z-axis. Range: 0° to 360°. Values less than 360° produce a pie-slice wedge. |

---

## Advanced usage

### Triaxial ellipsoids

Setting all three radii to different non-zero values produces a fully triaxial
ellipsoid — no axis of rotational symmetry. This is useful for organic shapes
such as the canopy of a helmet or a fruit silhouette. Radius3 = 0 is a special
shorthand for Radius3 = Radius2; any positive non-zero value activates the
independent Y-axis scaling.

### Combining clipping angles for lens and crescent sections

By setting Angle1 and Angle2 to values in the same hemisphere (e.g., both
positive), you obtain a band or zone cut from the middle of the ellipsoid. Pairing
this with Angle3 < 360° produces complex partial shapes that would be tedious to
model with a revolved sketch.

### Attachment to planes and edges

Like all PartDesign primitives, the ellipsoid inherits the `Part::AttachExtension`
and can be repositioned using **Part Design → Attachment** to snap its coordinate
system to any face, edge, or vertex in the Body.

---

## Common mistakes and pitfalls

!!! warning "Ellipsoid appears as a thin ring or a very flat disc"
    **Cause:** Angle1 and Angle2 are too close together, leaving only a narrow
    latitude band.  
    **Fix:** Widen the angle range — set Angle1 to a negative value and Angle2 to
    a positive value that spans the intended portion of the ellipsoid.

!!! warning "Shape is not fused to the existing Body solid"
    **Cause:** The ellipsoid does not intersect or touch the current tip of the
    Body, so the Boolean fusion leaves a disconnected lump.  
    **Fix:** Move the Body origin, use the Attachment tool to reposition the
    primitive, or place it so that it at least touches the existing solid.

!!! warning "Radius3 has no visible effect"
    **Cause:** Radius3 is set to a non-zero value but it is very close to Radius2,
    so the deformation is imperceptible in the viewport.  
    **Fix:** Set a value significantly different from Radius2 (for example, half or
    double) to see the Y-axis stretching clearly. Also verify the value was
    committed — press Enter after typing.

!!! warning "Partial sweep (Angle3 < 360°) has an open face"
    **Cause:** A sweep angle less than 360° is geometrically valid but leaves two
    flat cut faces. This is expected behaviour; the solid is still closed and
    watertight.  
    **Fix:** This is not an error. If you need a closed shape, set Angle3 to 360°
    or fuse additional features to cover the open faces.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

ellipsoid = doc.addObject("PartDesign::AdditiveEllipsoid", "Ellipsoid")
body.addObject(ellipsoid)

# Default full ellipsoid — Radius1=2, Radius2=4, Radius3=0 (= Radius2)
# Angle1=-90°, Angle2=90°, Angle3=360°
doc.recompute()

print(f"Volume: {ellipsoid.Shape.Volume:.4f} mm³")
# For a prolate ellipsoid with R1=2, R2=4: V = (4/3)π * 4 * 4 * 2 ≈ 134.04 mm³
# TODO: verify computed volume matches source formula
```

### Customising radii and angles

```python
ellipsoid.Radius1 = 5.0    # Z-axis (polar)
ellipsoid.Radius2 = 10.0   # X-axis (equatorial)
ellipsoid.Radius3 = 7.0    # Y-axis (independent)
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

- [Additive Sphere](additive-sphere.md) — simpler primitive for a perfect sphere
- [Subtractive Ellipsoid](subtractive-ellipsoid.md) — same parameters, removes material
- [Additive Box](additive-box.md) — rectangular block primitive
- [Additive Torus](additive-torus.md) — donut-shaped primitive with similar angle parameters
- [Revolution](revolution.md) — revolve a custom sketch profile for non-standard ellipsoidal shapes
