# Subtractive Sphere

> **In one sentence:** Cut a spherical or partial-spherical cavity out of an existing solid in a Part Design Body without needing a sketch.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Sphere  
**Toolbar:** Part Design Modeling Primitives (CompPrimitiveSubtractive) · **Shortcut:** none

Subtractive Sphere creates a parametric sphere — defined by a radius and three optional clipping angles — and subtracts it from the active Body's current solid using a boolean cut. It is the subtractive counterpart of [Additive Sphere](additive-sphere.md) and shares identical parameters. No sketch is required: the sphere is placed and sized immediately through a task panel.

---

## Intuition

Imagine scooping a perfectly round hollow out of a block of material with an ice-cream scoop. The size of the scoop is the Radius. The result is a smooth, curved cavity whose walls are all equidistant from the centre point.

Three angle parameters give you finer control over the shape of the cut:

- **Angle1** and **Angle2** clip the sphere with horizontal planes (latitude cuts). They default to −90° and +90°, giving a complete sphere. Narrowing this range produces a truncated sphere — useful for dome-shaped sockets or lenticular cutouts.
- **Angle3** clips the sphere with a vertical plane sweep (longitude cut). It defaults to 360°, giving a complete revolution. Reducing it produces a partial sphere sector — a wedge-shaped hollow.

At default angles the result is a full spherical cavity. Adjust the angles only when you need a partial form.

---

## When to use it

- You need a hemispherical or spherical socket, seat, or dome-shaped cavity in an existing solid.
- You are designing a ball-joint recess, a spherical bearing housing, or a mould cavity for a round object.
- You want to introduce a smooth, curved hollow without drawing and revolving a sketch.
- You need a partial spherical cutout — a dome cap, a lens-shaped recess, or a sector cut — using the angle parameters.
- You are scripting models and want a single API call to produce a spherical cut.

---

## When NOT to use it

- **The cavity is not spherical** — for toroidal, ellipsoidal, or freeform curved cuts, use the appropriate subtractive primitive or a revolved [Pocket](pocket.md) with a sketch profile.
- **You need the cut to follow a pattern** — use [Pocket](pocket.md) with a revolved sketch, which integrates more naturally with Part Design pattern tools (Polar Pattern, Linear Pattern).
- **You want to add spherical material** — use [Additive Sphere](additive-sphere.md), which has identical parameters but fuses instead of cuts.
- **No solid exists in the Body yet** — a subtractive feature requires an existing base feature to subtract from.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one existing additive feature (the solid to cut into).

1. **Activate Part Design.** Open or switch to the Part Design workbench.

2. **Invoke the tool.** Go to **Part Design → Subtractive Primitives → Subtractive Sphere**. Alternatively, click the Subtractive Sphere icon in the toolbar (inside the CompPrimitiveSubtractive drop-down button).

3. **The task panel opens** showing Radius, Angle1, Angle2, and Angle3 fields. Defaults are Radius = 5 mm, Angle1 = −90°, Angle2 = 90°, Angle3 = 360°. The 3-D view shows the sphere as a preview shape.

4. **Set the radius.** Enter the desired sphere radius.

5. **Adjust the angles if needed.** For a full spherical cavity, leave all angles at their defaults. To produce a dome-shaped cut (flat bottom), set Angle1 to 0° and keep Angle2 at 90°. To produce a partial sector, reduce Angle3 below 360°.

6. **Optionally set an attachment.** Use the **Attachment** section to position the sphere centre relative to an existing face, edge, vertex, or datum plane.

7. **Click OK.** The sphere is subtracted from the Body.

!!! tip
    The sphere centre is placed at the attachment origin. To sink the sphere so only the upper hemisphere is cut (a dome socket), attach the sphere to the target face and apply an **Attachment offset** of `−Radius` along the attachment Z-axis, so the equator sits flush with the face.

!!! warning "No base feature available"
    If the Body has no tip feature, FreeCAD shows a warning and cancels. Add at least one additive feature first.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius | Length (mm) | 5.0 mm | Radius of the sphere. Must be greater than zero. |
| Angle1 | Angle (°) | −90.0° | Lower latitude clipping angle. Range: −90° to +90°. The default −90° includes the south pole. Increasing Angle1 clips the bottom of the sphere. |
| Angle2 | Angle (°) | 90.0° | Upper latitude clipping angle. Range: −90° to +90°. The default +90° includes the north pole. Decreasing Angle2 clips the top of the sphere. Angle2 must be greater than Angle1. |
| Angle3 | Angle (°) | 360.0° | Sweep angle around the vertical axis (longitude). Range: 0° < Angle3 ≤ 360°. The default 360° gives a complete revolution. Values below 360° produce a partial spherical sector. |

The task panel also exposes the **Attachment** section (inherited from `Part::AttachExtension`), which controls the position of the sphere centre. See the [Datum Plane](datum-plane.md) documentation for the full list of modes. <!-- TODO: verify link -->

---

## Advanced usage

### Hemisphere cuts

To cut only a hemisphere (half-sphere dome socket), set Angle1 to 0° and leave Angle2 at 90°. This keeps the upper half of the sphere and clips the lower half flat. Combined with an attachment offset equal to the radius, the flat face of the hemisphere aligns with the target face.

### Lenticular (lens-shaped) cavities

Set Angle1 to a positive value (e.g. 30°) and Angle2 to a smaller positive value (e.g. 60°) to produce a thin spherical cap cut — useful for optical lens seat cavities or lenticular engraving.

### Driving dimensions with expressions

Radius and all three angles accept FreeCAD expressions. Bind the radius to a spreadsheet cell (e.g. `=Params.BallRadius`) to create parametric ball-seat geometry that updates automatically when the ball diameter changes.

---

## Common mistakes and pitfalls

!!! warning "Sphere appears at the wrong location"
    **Cause:** The sphere centre is placed at the Body origin (or attachment origin) by default. If the origin is not where you want the cavity, the cut lands elsewhere.  
    **Fix:** Use the **Attachment** section to attach the sphere to the desired face and add an **Attachment offset** to fine-tune the centre position.

!!! warning "Radius too small — no visible cut"
    **Cause:** FreeCAD enforces a minimum positive radius. Setting Radius to zero or near-zero triggers an error (`Radius of sphere too small`). A very small radius may also produce a degenerate boolean result.  
    **Fix:** Ensure Radius is greater than zero and large enough to intersect the base solid.

!!! warning "Angle1 equals or exceeds Angle2"
    **Cause:** If Angle1 ≥ Angle2, the sphere degenerates to zero volume and the cut cannot be computed.  
    **Fix:** Ensure Angle1 is strictly less than Angle2. For a full sphere, use Angle1 = −90° and Angle2 = 90°.

!!! warning "Resulting shape is not a solid"
    **Cause:** The sphere is positioned entirely outside the base solid, so the boolean cut has no material to remove.  
    **Fix:** Check the attachment position so the sphere overlaps the existing solid.

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
base.Height = 40.0
doc.recompute()

# Create a Subtractive Sphere — full spherical cavity of radius 10 mm
cavity = doc.addObject("PartDesign::SubtractiveSphere", "SphereCavity")
body.addObject(cavity)

cavity.Radius = 10.0   # mm
cavity.Angle1 = -90.0  # degrees — south pole included
cavity.Angle2 =  90.0  # degrees — north pole included
cavity.Angle3 = 360.0  # degrees — full revolution

doc.recompute()

import math
sphere_vol = (4 / 3) * math.pi * 10**3
base_vol   = 40 * 40 * 40
print(f"Volume after cut: {cavity.Shape.Volume:.4f} mm³")
# Expected: 64000 − (4/3)π × 1000 ≈ 59810.3 mm³
```

### Resulting feature properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Radius` | `App::PropertyLength` | `5.0 mm` | Radius of the sphere. Must be > 0. |
| `Angle1` | `App::PropertyAngle` | `−90.0°` | Lower latitude clipping angle. Range: [−90°, +90°). |
| `Angle2` | `App::PropertyAngle` | `90.0°` | Upper latitude clipping angle. Range: (−90°, +90°]. Must be > Angle1. |
| `Angle3` | `App::PropertyAngle` | `360.0°` | Longitude sweep angle. Range: (0°, 360°]. |

Properties inherited from `PartDesign::FeatureAddSub`:

| Property | Type | Description |
|----------|------|-------------|
| `BaseFeature` | `App::PropertyLink` | The feature this cut is applied to. |
| `AddSubShape` | `Part::PropertyPartShape` | The raw sphere shape before the boolean cut. |

---

## See also

- [Additive Sphere](additive-sphere.md) — identical parameters, adds spherical material instead of removing it
- [Pocket](pocket.md) — sketch-based cut with a revolved profile; more flexible for non-spherical curved cuts
- [Subtractive Box](subtractive-box.md) — rectangular cut primitive
- [Subtractive Cylinder](subtractive-cylinder.md) — cylindrical cut primitive
- [Subtractive Cone](subtractive-cone.md) — conical/frustum cut primitive
