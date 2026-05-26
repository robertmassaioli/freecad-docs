# Additive Helix

> **In one sentence:** Sweep a closed 2-D profile sketch along a helical path
> to create a coil, thread, or spiral solid and fuse it into the active Body.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Features → Additive Helix  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Additive Helix takes a closed sketch and sweeps it along an automatically
generated helix, producing a solid coil or thread form that is fused into the
active Body. Unlike Additive Pipe, you do not need to draw the helical path
yourself — the tool computes it from four geometric parameters (pitch, height,
turns, and cone angle) that you supply in four convenient input modes. Use it
for coil springs, screw threads, worm gears, or any other solid with a helical
geometry.

---

## Intuition

Think of wrapping a ribbon around a cylinder: the ribbon is the *profile* and
the cylinder's surface traces the *helix*. The tightness of the wrapping is the
*pitch* (axial distance per turn), the height of the cylinder is the total
*height* of the helix, and the number of times the ribbon goes around is the
*turns* count.

What makes Additive Helix special compared to Additive Pipe with a manually
drawn spine is that the helix geometry is fully parametric and self-consistent:
change the pitch and the turns count updates, change the height and the pitch
recalculates — depending on which *mode* you choose to drive the geometry.

For conical spirals (like a watch spring or a decreasing-pitch compression
spring), a non-zero *Cone angle* makes the radius grow or shrink as the helix
climbs.

---

## When to use it

- You need a coil spring — a circular profile swept along a cylindrical helix.
- You want to add a helical thread form to a shaft (Additive Helix adds the
  thread material; use [Subtractive Helix](subtractive-helix.md) to cut a
  thread groove).
- You are modelling a worm gear tooth, an Archimedes screw, or any helically
  extruded feature.
- You need a conical spiral where the radius changes with each turn.
- You need a flat Archimedean spiral (set height to zero and use
  Height-Turns-Growth mode with a non-zero growth value).

---

## When NOT to use it

- **The path is an arbitrary curve, not a helix** — use
  [Additive Pipe](additive-pipe.md) with a manually drawn spine.
- **The cross-section is constant and the path is straight** — use
  [Pad](pad.md).
- **The shape is axisymmetric** — use [Revolution](revolution.md).
- **You want to cut a helical groove** — use
  [Subtractive Helix](subtractive-helix.md), which has identical parameters but
  removes material.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one closed, fully-constrained
sketch. The sketch should be placed on a plane that is perpendicular to (or
near) the desired helix axis, and it should be offset from that axis (centred
sketches produce self-intersecting geometry).

1. **Select the profile sketch** in the model tree or viewport.

2. **Activate Part Design → Additive Features → Additive Helix.** The task
   panel opens. FreeCAD automatically proposes a pitch and height based on the
   bounding box of your profile sketch to avoid self-intersection.

3. **Choose the Axis.** The dropdown lists the Body origin axes (X, Y, Z),
   the sketch's horizontal, vertical, and normal axes, and any custom reference.
   The sketch's normal axis (perpendicular to its plane) is often the right
   choice.

4. **Choose the Mode.** Pick the combination of inputs that matches what you
   know about your desired helix. See the [Parameters](#parameters) table for
   a description of each mode.

5. **Fill in the primary inputs** for the chosen mode (e.g., Pitch and Height
   for Pitch-Height-Angle mode). The derived parameters are computed and
   displayed as read-only values.

6. **Set the Cone angle** if you want a conical spiral (positive = radius
   grows upward, negative = radius shrinks). Leave at 0° for a cylindrical
   helix.

7. **Check "Left handed"** to reverse the chirality from right-handed
   (clockwise when looking along the axis) to left-handed (counter-clockwise).

8. **Check "Reversed"** to make the helix travel in the opposite direction
   along the axis.

9. **Click OK.** FreeCAD sweeps the profile along the helix and fuses the
   result into the Body.

!!! tip
    If the task panel shows a "self-intersecting" status warning, increase the
    pitch relative to the profile size. The profile's cross-section must fit
    inside the axial distance between turns to avoid overlapping geometry.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Axis | Dropdown | (Body origin Y-axis) <!-- TODO: verify default axis --> | The central axis of the helix. Choices: Body X/Y/Z axes, sketch horizontal/vertical/normal axes, or an arbitrary reference. |
| Mode | Dropdown | `Pitch-Height-Angle` | Input mode. Determines which three parameters are primary inputs and which are derived. See mode table below. |
| Pitch | Length (mm) | 10.0 mm | Axial distance between two adjacent turns. Primary input in `Pitch-Height-Angle` and `Pitch-Turns-Angle` modes; derived in others. Must be > 0 when it is a primary input. |
| Height | Length (mm) | 30.0 mm | Total axial length of the helix path (does not include the extent of the profile itself). Primary input in `Pitch-Height-Angle`, `Height-Turns-Angle`, and `Height-Turns-Growth` modes; derived in `Pitch-Turns-Angle` mode. |
| Turns | Float | 3.0 | Number of full turns. Primary input in `Pitch-Turns-Angle`, `Height-Turns-Angle`, and `Height-Turns-Growth` modes; derived in `Pitch-Height-Angle` mode. Minimum value: `Precision::Confusion()` (effectively > 0). |
| Cone angle | Angle (°) | 0° | Half-angle of the cone that envelopes the helix. `0°` = cylindrical helix. Positive = radius grows in the axis direction; negative = radius shrinks. Valid range: −89° to +89°. |
| Radial growth | Length (mm) | 0.0 mm | Radius increase per turn. Primary input only in `Height-Turns-Growth` mode; derived from `Cone angle` in all other modes. |
| Left handed | Checkbox | Off | When on, the helix winds counter-clockwise when viewed along its axis (left-hand thread convention). |
| Reversed | Checkbox | Off | When on, the helix travels in the opposite direction along the axis. |
| Tolerance | Float | 0.1 | Fusion tolerance multiplier. Increase if the helical solid does not merge cleanly with the existing Body. Minimum 0.1. |

**Input modes:**

| Mode | Primary inputs | Derived |
|------|---------------|---------|
| **Pitch-Height-Angle** | Pitch, Height, Cone angle | Turns, Radial growth |
| **Pitch-Turns-Angle** | Pitch, Turns, Cone angle | Height, Radial growth |
| **Height-Turns-Angle** | Height, Turns, Cone angle | Pitch, Radial growth |
| **Height-Turns-Growth** | Height, Turns, Radial growth | Pitch, Cone angle |

!!! note
    The `Outside` (Remove outside of profile) property is present in the source
    code but is hidden for Additive Helix — it only applies to Subtractive Helix,
    where it controls whether the material inside or outside the swept volume is
    removed.

---

## Advanced usage

### Flat Archimedean spirals

To create a flat spiral (zero height), use **Height-Turns-Growth** mode and set
Height to 0. The result is a flat disc-like spiral whose radius increases by
the `Radial growth` value each turn. This is useful for scroll springs or
decorative spiral features. Note that with zero height and more than one turn,
the helix path collapses onto a plane, which may require adjusting the Tolerance
parameter for the fusion to succeed.

### Self-intersection avoidance

FreeCAD proposes an initial pitch based on the profile bounding box (roughly
1.1 × the bounding-box diagonal). This is a conservative estimate — you can
usually reduce the pitch significantly for compact profiles. The status label at
the top of the task panel shows "Valid" when the proposed geometry is
self-intersection-free and an error message when it is not.

### Thread profiles

For external threads, draw the thread-tooth profile as a closed triangular or
trapezoidal sketch on a plane perpendicular to the shaft axis, positioned so
that the sketch is at the thread's outer diameter. Use the shaft's Z-axis as
the helix axis. Set the pitch to match the thread standard (e.g. 1.0 mm for
M6×1.0). After the Additive Helix, use a [Revolution](revolution.md) or
[Pad](pad.md) to add the core shaft body.

### Conical coil springs

A non-zero Cone angle produces a conical coil spring (like a tapered
compression spring). Positive angles make the coil wider towards the top of
the axis direction; negative angles narrow it. The cone angle is the half-angle
of the imaginary cone that the helix wraps around — a 10° cone angle on a 5 mm
radius spring increases the radius by about 0.88 mm per 5 mm of height.

---

## Common mistakes and pitfalls

!!! warning "Error: Pitch too small"
    **Cause:** In `Pitch-Height-Angle` or `Pitch-Turns-Angle` mode the pitch
    value is zero or below the geometric precision threshold.  
    **Fix:** Set Pitch to a positive value larger than the axial extent of your
    profile cross-section to avoid self-intersection.

!!! warning "Result is self-intersecting"
    **Cause:** The profile is too large relative to the pitch, so adjacent turns
    of the sweep overlap. The status label in the task panel will show a warning.  
    **Fix:** Reduce the profile cross-section size, increase the pitch, or use
    a larger axis offset (move the sketch further from the axis).

!!! warning "Error: Face must be planar"
    **Cause:** The profile sketch is attached to a non-planar face. The helix
    sweep algorithm requires a planar starting face.  
    **Fix:** Attach the profile sketch to a flat datum plane or a planar model
    face.

!!! warning "Adding the helix failed (fusion error)"
    **Cause:** The swept solid does not cleanly merge with the existing Body,
    often because of very tight geometry or a large model with small helix
    features.  
    **Fix:** Increase the `Tolerance` parameter in the task panel. Values above
    1.0 are sometimes necessary for very large parts.

!!! warning "Helix direction looks wrong"
    **Cause:** The axis direction or the Reversed flag may be producing a helix
    that climbs in the opposite direction from what was intended.  
    **Fix:** Toggle the **Reversed** checkbox, or choose a different axis
    (e.g., swap from +Z to −Z direction by selecting the opposite origin axis).

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create the body
body = doc.addObject("PartDesign::Body", "Body")

# Profile sketch: a small circle of radius 2 mm on the XY plane
# The circle is offset from the origin so it does not intersect the Z-axis
profile = doc.addObject("Sketcher::SketchObject", "Profile")
body.addObject(profile)
profile.addGeometry(Part.Circle(App.Vector(10, 0, 0), App.Vector(0, 0, 1), 2), False)
profile.addConstraint(Sketcher.Constraint("DistanceX", -1, 1, 0, 3, 10))  # TODO: verify
doc.recompute()

# Create the Additive Helix
helix = doc.addObject("PartDesign::AdditiveHelix", "AdditiveHelix")
body.addObject(helix)
helix.Profile = profile
helix.ReferenceAxis = (doc.getObject("Body").Origin.Z_Axis, [""])  # TODO: verify axis reference
helix.Mode = "pitch-height-angle"
helix.Pitch = 5.0      # mm per turn
helix.Height = 30.0    # total axial length
helix.Angle = 0.0      # cylindrical (no cone)
helix.LeftHanded = False
helix.Reversed = False
doc.recompute()

print(f"Volume: {helix.Shape.Volume:.4f} mm³")
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The cross-section sketch to sweep. Inherited from `ProfileBased`. |
| `ReferenceAxis` | `App::PropertyLinkSub` | — | The axis object and optional sub-element. When set, `Base` and `Axis` are derived automatically. |
| `Mode` | `App::PropertyEnumeration` | `"pitch-height-angle"` | Input mode. Values: `"pitch-height-angle"`, `"pitch-turns-angle"`, `"height-turns-angle"`, `"height-turns-growth"`. |
| `Pitch` | `App::PropertyLength` | `10.0` | Axial distance per turn in mm. |
| `Height` | `App::PropertyLength` | `30.0` | Total axial length of the helix path in mm. |
| `Turns` | `App::PropertyFloatConstraint` | `3.0` | Number of full turns. Must be > 0. |
| `Angle` | `App::PropertyAngle` | `0.0` | Cone half-angle in degrees. Range: −89° to +89°. |
| `Growth` | `App::PropertyDistance` | `0.0` | Radial growth per turn in mm. Used in `"height-turns-growth"` mode; otherwise derived. |
| `LeftHanded` | `App::PropertyBool` | `False` | When `True`, helix winds counter-clockwise (left-hand thread direction). |
| `Reversed` | `App::PropertyBool` | `False` | Reverses the axial direction of the helix. |
| `Tolerance` | `App::PropertyFloatConstraint` | `0.1` | Fusion tolerance multiplier. Minimum 0.1. |
| `Base` | `App::PropertyVector` | `(0,0,0)` | Start point of the helix axis. Read-only; derived from `ReferenceAxis`. |
| `Axis` | `App::PropertyVector` | `(0,1,0)` | Direction of the helix axis. Read-only; derived from `ReferenceAxis`. |

---

## See also

- [Subtractive Helix](subtractive-helix.md) — identical parameters, cuts a helical groove instead
- [Additive Pipe](additive-pipe.md) — sweep along an arbitrary user-drawn path
- [Revolution](revolution.md) — simpler rotation for axisymmetric shapes
- [Additive Loft](additive-loft.md) — blend through multiple cross-sections
- [Pad](pad.md) — straight extrusion for constant cross-sections
