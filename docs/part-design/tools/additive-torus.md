# Additive Torus

> **In one sentence:** Add a solid torus — a donut or ring shape — directly into
> a Part Design Body, fusing it with any existing material.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Torus  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Additive Torus inserts a parametric torus solid into the active Body and fuses it
with the existing shape. The torus is defined by two radii — the distance from the
centre of the ring to the centre of the tube (Radius1) and the radius of the tube
itself (Radius2) — plus three angular clipping parameters. These angles let you
produce everything from a full donut to a curved tube segment, a split ring, or a
partial torus sector. No sketch is required; the shape is fully defined by its
five numeric properties.

---

## Intuition

Picture a donut lying flat on a table. The distance from the centre of the donut
to the centre of its dough is **Radius1** — the "major" or ring radius. The
thickness of the dough — measured from its centre to its surface — is **Radius2**,
the tube radius.

The two angle parameters Angle1 and Angle2 slice the tube cross-section like
cutting the dough ring with two parallel horizontal planes. With the default values
(−180° and +180°) you get the full tube profile, giving a closed circular
cross-section. Tightening the range, for example to 0° and 180°, trims the tube
to a half-circle, producing a channel-shaped section.

Angle3 controls how far around the Z-axis the ring extends — `360°` gives the
full closed donut, while a smaller value such as `90°` gives a quarter-arc curved
tube section. This is the quickest way to model a bent pipe or a curved bracket
without drawing any sketches.

When Radius2 equals Radius1, the inner hole of the torus closes entirely and you
get a shape that pinches to a point at the centre — a horn torus. If Radius2
exceeds Radius1, the shape self-intersects and FreeCAD may fail to compute it.

---

## When to use it

- You need a seal, gasket, O-ring, or decorative ring and want to set the exact
  tube and ring radii numerically.
- You are modelling a bent pipe, elbow, or curved channel and want a fixed
  circular cross-section on the sweep.
- You need a partial torus arc — for example, a 90° elbow or a 180° half-ring
  handle — using Angle3 less than 360°.
- You want a rounded ridge or fillet-like raised ring on a cylindrical surface
  without constructing a profile sketch.

---

## When NOT to use it

- **The cross-section of the sweep is not circular** — use
  [Additive Pipe](additive-pipe.md) with a custom profile sketch instead.
- **The sweep path is not a circle or arc** — use [Additive Pipe](additive-pipe.md)
  with a spine sketch for arbitrary curved paths.
- **You want to remove a toroidal pocket** — use
  [Subtractive Torus](subtractive-torus.md), which has identical parameters but
  cuts instead of adds.
- **The torus represents a thread** — use
  [Additive Helix](additive-helix.md) for helical sweeps or the dedicated Thread
  tool if available.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body in the document. The torus can be
the very first feature (no existing solid required).

1. Open or create a document and ensure a Body is active. FreeCAD will offer to
   create one automatically if none exists when you invoke the command.

2. Activate **Part Design → Additive Primitives → Additive Torus** from the menu,
   or click its icon in the Part Design Modeling Primitives toolbar. If multiple
   primitives share one toolbar button, expand the drop-down and choose
   **Additive Torus**.

3. The task panel opens. Set **Radius1** to the distance from the torus centre to
   the centre of the tube. Set **Radius2** to the tube's own radius. Ensure
   Radius2 is smaller than Radius1 to avoid self-intersection.

4. Adjust **Angle1** and **Angle2** if you need a partial tube cross-section
   (for example, a half-round channel). Leave them at −180° / +180° for the
   standard solid circular tube.

5. Reduce **Angle3** from 360° if you need only a portion of the ring arc — useful
   for elbows, handles, or decorative arcs.

6. Click **OK**. The torus is fused into the Body and appears in the model tree
   as **Torus**.

!!! tip
    For a 90° pipe elbow: set Radius1 to the bend radius of your pipe centreline
    and Radius2 to the pipe's outer radius, then set Angle3 to 90°. The result
    is a solid elbow ready to be Boolean-cut or joined with straight cylinders at
    each end.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Length (mm) | 10.0 | Distance from the torus centre to the centre of the tube (major radius). Must be > 0. |
| Radius2 | Length (mm) | 2.0 | Radius of the tube itself (minor radius). Must be > 0 and should be ≤ Radius1 to avoid self-intersection. |
| Angle1 | Angle (°) | −180.0 | Start angle of the tube cross-section profile. Range: −180° to +180°. Controls where the tube profile begins (latitude of the meridian arc). |
| Angle2 | Angle (°) | 180.0 | End angle of the tube cross-section profile. Range: −180° to +180°. Must be greater than Angle1. The default pair −180°/+180° gives a full circular tube. |
| Angle3 | Angle (°) | 360.0 | Sweep angle around the Z-axis (longitude of the ring). Range: 0° to 360°. Values less than 360° produce a partial arc. |

---

## Advanced usage

### Partial torus arc (elbow / curved channel)

Setting Angle3 to any value less than 360° cuts the ring to an arc. The open ends
are flat faces, so you can mate straight cylinders or pipes to them. A common
workflow:

1. Set Radius1 = bend centreline radius, Radius2 = tube outer radius.
2. Set Angle3 = desired sweep angle (e.g., 45°, 90°, 180°).
3. Use [Additive Cylinder](additive-cylinder.md) features at each end to extend
   the straight sections.

### Half-round channel

Setting Angle1 = 0° and Angle2 = 180° with Angle3 = 360° produces a ring with a
semicircular cross-section — open at the bottom like a channel. This is useful for
decorative mouldings or seal grooves where the cross-section must be a half-round.

### Approaching a horn torus

When Radius2 approaches Radius1, the inner hole of the torus closes to a circle.
This transition can produce a smooth spindle or horn shape useful as an organic
design element. Be aware that as Radius2 gets very close to or exceeds Radius1 the
OCCT kernel may fail to generate a valid solid; keep Radius2 slightly less than
Radius1 for safe computation.

---

## Common mistakes and pitfalls

!!! warning "Shape fails to compute or produces a degenerate solid"
    **Cause:** Radius2 is equal to or greater than Radius1. The inner ring folds
    into itself and OCCT cannot construct a valid closed solid.  
    **Fix:** Ensure Radius2 < Radius1. A typical safe ratio is Radius2 ≤ 0.9 ×
    Radius1.

!!! warning "Angle1 and Angle2 appear to do nothing when Angle3 = 360°"
    **Cause:** When the tube profile angles are both at their defaults (−180° /
    +180°), the cross-section is fully circular. Changing them narrows the cross-
    section but may not be obvious in the viewport at small torus sizes.  
    **Fix:** Set Angle1 = 0° and Angle2 = 90° and zoom into the cross-section face
    to confirm the effect, or check the Shape volume in the Model panel.

!!! warning "Partial torus (Angle3 < 360°) is not connected to the Body"
    **Cause:** The arc does not touch the existing solid, so the fusion has no
    effect and the result is a disconnected shape.  
    **Fix:** Use the Attachment tool or reposition the Body origin so the torus
    overlaps the existing geometry before fusing.

!!! warning "Torus appears inside-out or inverted in a Boolean operation"
    **Cause:** The normal direction of the torus faces is outward by OCCT
    convention; if the torus is fully enclosed inside another solid, the Boolean
    may behave unexpectedly.  
    **Fix:** For cutting a toroidal channel into a solid, use
    [Subtractive Torus](subtractive-torus.md) rather than placing an additive one
    inside the solid and expecting a cavity.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

torus = doc.addObject("PartDesign::AdditiveTorus", "Torus")
body.addObject(torus)

# Default full torus: Radius1=10, Radius2=2, Angle1=-180°, Angle2=180°, Angle3=360°
doc.recompute()

print(f"Volume: {torus.Shape.Volume:.4f} mm³")
# V = 2π² * R1 * R2² = 2π² * 10 * 4 ≈ 789.57 mm³  (full torus)
# TODO: verify computed volume matches analytic formula
```

### Customising the torus

```python
torus.Radius1 = 20.0   # ring centreline radius
torus.Radius2 = 4.0    # tube radius
torus.Angle1  = 0.0    # half-round cross-section (lower open)
torus.Angle2  = 180.0
torus.Angle3  = 90.0   # quarter-arc only
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Radius1` | `App::PropertyLength` | Major radius — distance from torus centre to tube centre, mm. |
| `Radius2` | `App::PropertyLength` | Minor radius — tube radius, mm. |
| `Angle1` | `App::PropertyAngle` | Start angle of tube cross-section, degrees. Range −180° to +180°. |
| `Angle2` | `App::PropertyAngle` | End angle of tube cross-section, degrees. Range −180° to +180°. |
| `Angle3` | `App::PropertyAngle` | Sweep angle around Z-axis, degrees. Range 0° to 360°. |

---

## See also

- [Subtractive Torus](subtractive-torus.md) — same parameters, cuts a toroidal cavity
- [Additive Pipe](additive-pipe.md) — sweep a custom profile along any path
- [Additive Helix](additive-helix.md) — helical sweep for springs and threads
- [Additive Cylinder](additive-cylinder.md) — straight cylindrical solid primitive
- [Additive Ellipsoid](additive-ellipsoid.md) — ellipsoidal solid with similar angle clipping
