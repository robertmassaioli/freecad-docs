# Subtractive Torus

> **In one sentence:** Cut a donut-shaped cavity out of an existing Part Design
> Body solid using a parametric torus defined by two radii and three angular
> clipping parameters.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Torus  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Subtractive Torus removes a toroidal volume from the current tip of the active
Body using a Boolean cut. The torus is defined by a major radius (distance from
the torus centre to the tube centreline) and a minor radius (tube cross-section
radius), plus three angular clipping parameters. The clipping parameters let you
cut partial tori — truncated arcs, wedge sectors, or band sections — without any
sketch work.

---

## Intuition

Imagine a rubber O-ring pressed into a block of material: it leaves a circular
groove whose cross-section is a circular arc. Subtractive Torus automates exactly
this operation — you specify the size of the O-ring (Radius1 = ring diameter,
Radius2 = tube thickness) and FreeCAD cuts it out.

Radius1 is the distance from the torus's central axis to the centre of the tube
— the "ring size". Radius2 is the radius of the tube itself — the "thickness".
A classic donut shape results when both Radius1 and Radius2 are positive and
Radius1 > Radius2.

Angle1 and Angle2 clip the tube's cross-section. They behave like latitude cuts
on the tube's circular cross-section: the default `−180° / +180°` keeps the full
circular tube; setting `Angle1 = 0°` removes the lower half of the tube,
leaving a D-shaped groove. Angle3 controls the sweep around the torus's central
Z-axis: `360°` gives a complete ring cavity, while smaller values give a
partial arc groove.

---

## When to use it

- You need a circular groove or O-ring seat machined into a flat face or flange
  without drawing and revolving a sketch.
- You want to model a toroidal pocket, such as a recessed ring channel in a
  hydraulic fitting or a decorative ring groove in a turned part.
- You need a partial arc groove (less than a full ring) using Angle3 < 360°.
- You are prototyping a concept and need to quickly try different torus
  proportions by adjusting two radii.

---

## When NOT to use it

- **You need to add a toroidal boss** — use [Additive Torus](additive-torus.md)
  instead; it has identical parameters but fuses material.
- **The groove profile is not circular** — use [Groove](groove.md) to revolve any
  custom sketch profile around an axis.
- **No solid exists yet in the Body** — subtractive primitives require an existing
  base feature. FreeCAD will display an error if the Tip is empty.
- **The ring must follow a non-circular path** — use
  [Subtractive Pipe](subtractive-pipe.md) with a circular profile swept along the
  desired path.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body with at least one existing solid
feature (the Tip must be non-empty).

1. Ensure the Body you want to modify is active. If it is not, double-click it in
   the model tree.

2. Activate **Part Design → Subtractive Primitives → Subtractive Torus** from the
   menu, or click its icon in the Part Design Modeling Primitives toolbar. If
   multiple primitives share one toolbar button, expand the drop-down and select
   **Subtractive Torus**.

3. The task panel opens showing the five torus parameters. A preview of the torus
   appears in the viewport.

4. Set **Radius1** to the desired ring radius (centre of Body origin to centre of
   tube) and **Radius2** to the tube radius. For an O-ring seat, Radius1 is the
   seat's pitch circle radius and Radius2 is half the O-ring cross-section
   diameter.

5. Adjust **Angle1** and **Angle2** if you want only part of the tube
   cross-section. The defaults `−180° / +180°` include the entire circular tube.

6. Reduce **Angle3** below `360°` if you need only a partial arc groove rather
   than a complete ring.

7. Click **OK**. FreeCAD performs the Boolean cut and updates the Body shape. The
   feature appears in the model tree as **Torus**.

!!! tip
    For a standard O-ring groove, set Angle1 = −180°, Angle2 = 180°, and
    Angle3 = 360°. Then set Radius1 to the groove's pitch circle radius (measured
    from the part's symmetry axis) and Radius2 to half the O-ring wire diameter.
    Use the Attachment tool if the groove must be centred on a face other than the
    XY plane.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Radius1 | Length (mm) | 10.0 | Major radius — distance from the torus central axis to the centre of the tube. Controls the overall ring size. Must be > 0. |
| Radius2 | Length (mm) | 2.0 | Minor radius — radius of the circular tube cross-section. Controls groove depth and width. Must be > 0. |
| Angle1 | Angle (°) | −180.0 | Start angle of the tube cross-section arc. Range: −180° to +180°. Combined with Angle2, clips the tube profile. |
| Angle2 | Angle (°) | 180.0 | End angle of the tube cross-section arc. Range: −180° to +180°. Must be greater than Angle1. |
| Angle3 | Angle (°) | 360.0 | Sweep angle around the torus central Z-axis. Range: 0° to 360°. Values less than 360° produce a partial arc groove. |

---

## Advanced usage

### Half-tube grooves

Setting `Angle1 = 0°` and `Angle2 = 180°` cuts only the upper half of the tube
cross-section, leaving a flat-bottomed semicircular groove — useful for
retaining ring channels where you need a flat seating surface. Similarly,
`Angle1 = −180°` and `Angle2 = 0°` cuts the lower half.

### Partial arc grooves (Angle3 < 360°)

Reducing Angle3 creates a groove that does not close on itself — for example,
a 270° arc groove for a partial snap-ring seat. The two end faces of the partial
torus are flat planes; combine the Subtractive Torus with a
[Pocket](pocket.md) at each end if you need to merge the groove into an
adjoining feature.

### Subtractive primitives vs. Groove

A Subtractive Torus is equivalent to a [Groove](groove.md) with a circular sketch
profile. For a standard O-ring seat the primitive is faster to set up; for a
trapezoidal or square groove profile (common in hydraulic fittings) you must use
Groove with a custom sketch. Both approaches are fully parametric.

---

## Common mistakes and pitfalls

!!! warning "Cannot subtract primitive feature without base feature"
    **Cause:** The Body's Tip is empty — no additive feature has been added yet.  
    **Fix:** Add at least one additive feature (Pad, Additive Box, etc.) before
    applying any subtractive primitive.

!!! warning "Torus groove does not appear on the expected face"
    **Cause:** The torus is centred on the Body's origin by default, which may not
    coincide with the face you want to groove.  
    **Fix:** Use the Attachment tool to align the torus's local Z-axis with the
    face normal of the target surface before clicking OK.

!!! warning "Radius2 is larger than Radius1 — torus self-intersects"
    **Cause:** When the tube radius (Radius2) exceeds the major radius (Radius1),
    the torus folds through itself, which can cause the OCCT Boolean operation to
    fail or produce unexpected geometry.  
    **Fix:** Ensure Radius2 < Radius1. For very large tubes relative to the ring,
    consider modelling the shape with a revolved sketch (Groove) for more
    predictable results.

!!! warning "Partial torus (Angle3 < 360°) leaves open ends"
    **Cause:** A sweep angle less than 360° is geometrically valid but leaves two
    flat cut faces at the start and end of the arc.  
    **Fix:** This is expected. The solid remains watertight as long as the partial
    torus is fully contained within the base solid. If the arc groove exits through
    a side wall, the result may be an open shell — ensure the torus is fully
    embedded in the base material.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

# Create a base cylinder to cut into
base = doc.addObject("PartDesign::AdditiveCylinder", "Cylinder")
body.addObject(base)
base.Radius = 15.0
base.Height = 10.0
doc.recompute()

# Cut an O-ring groove near the top of the cylinder
torus = doc.addObject("PartDesign::SubtractiveTorus", "Torus")
body.addObject(torus)

# Default groove: Radius1=10, Radius2=2, full ring
doc.recompute()

print(f"Volume: {torus.Shape.Volume:.4f} mm³")
# TODO: verify computed volume
```

### Customising the torus

```python
torus.Radius1 = 12.0    # pitch circle radius of the O-ring seat
torus.Radius2 = 1.5     # half the O-ring wire diameter
torus.Angle1  = -180.0  # full tube cross-section (default)
torus.Angle2  =  180.0  # full tube cross-section (default)
torus.Angle3  = 360.0   # complete ring groove
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Radius1` | `App::PropertyLength` | Major radius — ring size, mm. |
| `Radius2` | `App::PropertyLength` | Minor radius — tube cross-section radius, mm. |
| `Angle1` | `App::PropertyAngle` | Start angle of tube arc, degrees. Range −180° to +180°. |
| `Angle2` | `App::PropertyAngle` | End angle of tube arc, degrees. Range −180° to +180°. |
| `Angle3` | `App::PropertyAngle` | Sweep around Z-axis, degrees. Range 0° to 360°. |

---

## See also

- [Additive Torus](additive-torus.md) — same parameters, adds a toroidal boss instead of cutting a groove
- [Subtractive Ellipsoid](subtractive-ellipsoid.md) — ellipsoidal cavity with similar angle clipping
- [Groove](groove.md) — revolve a custom sketch profile to cut any axisymmetric pocket
- [Subtractive Cylinder](subtractive-cylinder.md) — simpler cylindrical bore without tube curvature
- [Pocket](pocket.md) — extrude a closed sketch downward to remove material
