# Revolution

> **In one sentence:** Revolve a closed 2-D profile sketch around an axis to
> create an axisymmetric solid and fuse it into the active Body.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Features → Revolve  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Revolution takes a closed sketch and spins it around a chosen axis by a given
angle, producing a solid of revolution. The result is fused into the active
Body, adding material. A full 360° sweep produces a completely closed solid
such as a cylinder, cone, or torus; partial angles produce wedge-like solids
with two flat cut faces. The tool shares its task panel and code with Groove,
which performs the same operation subtractively.

---

## Intuition

Think of a pottery wheel: a lump of clay (the *profile*) spins around the
wheel's axis, tracing out the final vase shape. The profile defines the outline
of one "slice" through the object, and Revolution sweeps that slice through
space to form the complete solid.

The profile does not need to be centred on the axis. Place the sketch on one
side and the revolved solid will be hollow on the inside, like a ring or pipe
flange. Move the sketch closer to the axis to get a denser solid; move it
further away to get a larger hollow.

One constraint that surprises new users: **the axis must not pass through the
interior of the sketch**. The sketch must be entirely on one side of the axis,
otherwise OCCT cannot form a valid solid and the operation will fail.

---

## When to use it

- You are modelling any rotationally symmetric part: shafts, flanges, pulleys,
  knobs, washers, vases, or bottle shapes.
- You need to create a solid of revolution whose cross-section profile is
  non-trivial (a simple circle gives you a cylinder, but you can also sketch
  stepped or tapered profiles).
- You want the revolution to span only a partial angle — for example, a 90°
  bracket that is the quarter of a ring.
- You are building a parametric model where the outer diameter, inner diameter,
  or wall profile must be driven by named dimensions in a sketch.

---

## When NOT to use it

- **The sketch does not revolve around an axis** — use [Pad](pad.md), which
  extrudes straight, or [Additive Pipe](additive-pipe.md) for curved paths.
- **You want to remove material in a rotational pattern** — use
  [Groove](groove.md), the subtractive counterpart to Revolution.
- **The profile must follow a helical path** — use
  [Additive Helix](additive-helix.md) instead.
- **The shape blends smoothly between different cross-sections** — use
  [Additive Loft](additive-loft.md).

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one closed, fully-constrained
sketch. If no sketch is pre-selected, the task panel will prompt you to select
one.

1. **Select the profile sketch** in the model tree or viewport (or activate it
   so it is the most-recent feature in the Body).

2. **Activate Part Design → Additive Features → Revolve.** The task panel
   opens showing *Type*, *Axis*, *Angle*, and checkbox options.

3. **Choose the Type.** "Angle" is the default and is the most common. See the
   [Parameters](#parameters) table for all options.

4. **Choose the Axis.** The dropdown lists the Body's origin axes (X, Y, Z),
   the sketch's own horizontal and vertical axes, and any construction lines in
   the sketch. Select "Select reference…" to pick an edge or datum line from
   the viewport.

5. **Set the Angle** (for "Angle" type). The default is 360°. Enter any value
   from 0° (exclusive) to 360° (inclusive).

6. **Check "Symmetric to plane"** if you want the revolution to extend equally
   on both sides of the sketch plane (each side gets half the total angle).

7. **Check "Reversed"** to flip the direction of revolution. The *Reversed*
   checkbox is automatically disabled when *Symmetric to plane* is active.

8. **Click OK.** The sketch is revolved around the chosen axis and fused into
   the Body.

!!! tip
    Construction lines inside a sketch appear as additional axis choices in the
    dropdown. Drawing a single horizontal construction line on the sketch and
    selecting it as the axis is a clean way to keep everything in one sketch.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Type | Dropdown | `Angle` | Revolution method. See modes table below. |
| Axis | Dropdown | (Body origin Y-axis) <!-- TODO: verify default axis --> | The axis of revolution. Choices: Body X/Y/Z axes, sketch horizontal/vertical axes, sketch construction lines, or an arbitrary reference selected from the viewport. |
| Angle | Angle (°) | 360° | Total sweep angle for "Angle" type. Valid range: 0° (exclusive) to 360° (inclusive). |
| 2nd angle | Angle (°) | 0° | Second sweep angle for "Two angles" type. The revolution extends `Angle` in one direction and `2nd angle` in the other. |
| Symmetric to plane | Checkbox | Off | When on, the revolution extends half of `Angle` on each side of the sketch plane. Mutually exclusive with `Reversed`. |
| Reversed | Checkbox | Off | Reverses the direction of revolution. Automatically disabled when "Symmetric to plane" is active. |
| Face | Object link | — | (Up to face type only) The target face at which the revolution stops. Click the **Face** button to enter face-selection mode, then click a face in the viewport. |

**Type modes:**

| Mode | Behaviour |
|------|-----------|
| **Angle** | Revolves by the specified `Angle`. The most common mode. |
| **To last** | Revolves until it meets the last face of the Body in that direction. |
| **To first** | Not yet implemented — disabled in the UI. |
| **Up to face** | Revolves until it meets the selected face. |
| **Two angles** | Revolves `Angle` forward and `2nd angle` in reverse, producing an asymmetric solid. |

### Type in depth

The Type dropdown determines when the swept solid terminates angularly.

#### Angle (default)

You specify the exact sweep in degrees. The solid grows from 0° to that angle
around the axis. Think of a pie slice — you choose how big a slice.

**Use this mode when:**
- You know the angular extent (360° for a full solid of revolution, 90° for a
  quarter-round, 45° for a wedge, etc.)
- You want to use **Symmetric to plane** to centre the arc about the sketch
- Fine angular control is needed (e.g. a 270° sweep for a recessed shelf)

**Watch out for:** At exactly 360° the solid is closed with no seam face. At
359.9° there is a thin seam face. If you get unexpected extra faces on your
model, check whether you intended exactly 360°.

#### Two angles

Revolves by one angle in the forward direction *and* a separate second angle in
the reverse direction from the sketch plane. The total solid spans both arcs.

**Use this mode when:**
- You need a wedge or arc that spans the sketch plane asymmetrically (e.g. 270°
  forward and 90° backward)
- The solid must extend in both directions without two separate Revolution
  features

**Watch out for:** The two angles may not sum to more than 360° — OCCT cannot
build a solid that overlaps itself. Also, "forward" is defined by the axis and
sketch orientation; use the live preview to confirm which direction is Side 1.

#### To last

The revolution sweeps until it meets the geometrically farthest face of the
Body along the rotation arc.

**Use this mode when:**
- The Body already has geometry and you want the revolution to fill the remaining
  angular space up to the last wall
- The final angular extent is defined by existing geometry rather than a number

**Watch out for:** "Last" means the farthest face the swept volume can reach
within the Body. In complex models with internal pockets or features this can be
an unexpected face. Prefer Up to face when you need to target a specific surface.

#### To first

Intended to stop at the first face encountered along the rotation arc. **Not
implemented in FreeCAD 1.1** — the dropdown entry is disabled and cannot be
selected in the UI.

**Use this mode when:** Not available. Use **Up to face** as the alternative and
manually select the desired stopping face.

**Watch out for:** Attempting to set this via the Python API (`Type = "UpToFirst"`)
raises a recompute error: *"Revolve up to first is not yet supported"*.

#### Up to face

The revolution terminates at a specific face you select. The swept solid conforms
to that face's shape.

**Use this mode when:**
- The revolution must stop at a conical, spherical, or otherwise non-flat
  termination face
- The endpoint must track a named face parametrically as the model changes

**Watch out for:** The selected face must actually be intersected by the swept
volume. A face that lies entirely outside the swept arc, or is parallel to the
rotation axis and never crossed, will cause a "Feature could not be computed"
error.

!!! warning "To first is disabled"
    The **To first** mode appears in the dropdown but is currently disabled and
    cannot be selected in the UI. Attempting to set it via the Python API returns:
    *"Revolve up to first is not yet supported"*. Use **Up to face** instead.

---

## Advanced usage

### Using a sketch construction line as the axis

A construction line drawn inside the profile sketch automatically appears in
the Axis dropdown as "Construction line 1" (and so on for additional lines).
This is useful when the axis of revolution is geometrically related to the
profile — for example, the centreline of a hollow boss can be both a sketch
edge and the revolution axis, so changing one dimension updates both.

### Asymmetric revolutions with Two angles

"Two angles" mode lets you revolve by different amounts on each side of the
sketch plane. For example, `Angle = 270°` and `2nd angle = 90°` gives a
full 360° solid, but the "seam" is positioned differently from a straight
360° revolution. More practically, `Angle = 60°` and `2nd angle = 30°`
produces a 90° wedge that is not centred on the sketch plane.

### Combining with Symmetric to plane

"Symmetric to plane" divides `Angle` equally on both sides. A 180° symmetric
revolution produces a full half-sphere centred on the sketch plane rather than
a 180° arc starting from it. This is often the right choice for features that
must be centred on a mid-plane.

---

## Common mistakes and pitfalls

!!! warning "Revolve axis intersects the sketch"
    **Cause:** The chosen axis passes through the interior of the profile sketch.
    OCCT cannot create a valid solid when the generating face is cut by the axis.  
    **Fix:** Move the sketch entirely to one side of the axis, or choose a different
    axis that does not cross the profile.

!!! warning "Sketch is not fully closed"
    **Cause:** The profile sketch has an open wire (a gap between two endpoints).
    Revolution requires a face, which requires a closed wire.  
    **Fix:** Open the sketch, close the gap, and ensure the Sketcher reports zero
    open constraints.

!!! warning "Angle of revolution too large"
    **Cause:** The `Angle` property exceeds 360°. This can happen when setting
    values via the Python API.  
    **Fix:** Keep `Angle` at or below 360°. Use "Two angles" mode if you need
    the solid to span more than 360° is impossible — one full revolution is the
    maximum.

!!! warning "Result has multiple solids"
    **Cause:** The revolution produced a geometry with more than one disconnected
    solid — typically because the profile sketch contains multiple closed wires
    that are not nested.  
    **Fix:** Simplify the sketch to a single closed wire, or enable "Allow
    Compound" in the active Body settings.

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

# Profile sketch: an L-shaped profile on the XZ plane
# (a rectangle offset from the Y-axis, which will be the revolution axis)
profile = doc.addObject("Sketcher::SketchObject", "Profile")
body.addObject(profile)
profile.MapMode = "FlatFace"
profile.AttachmentSupport = (doc.XZ_Plane, [""])
doc.recompute()

# Simple rectangle: x from 10 to 20, z from 0 to 30
profile.addGeometry(Part.LineSegment(App.Vector(10, 0, 0),  App.Vector(20, 0, 0)), False)
profile.addGeometry(Part.LineSegment(App.Vector(20, 0, 0),  App.Vector(20, 0, 30)), False)
profile.addGeometry(Part.LineSegment(App.Vector(20, 0, 30), App.Vector(10, 0, 30)), False)
profile.addGeometry(Part.LineSegment(App.Vector(10, 0, 30), App.Vector(10, 0, 0)), False)
# (constraints omitted for brevity)  # TODO: verify
doc.recompute()

# Create the Revolution
rev = doc.addObject("PartDesign::Revolution", "Revolution")
body.addObject(rev)
rev.Profile = profile
rev.ReferenceAxis = (doc.Y_Axis, [""])
rev.Angle = 360.0
rev.Midplane = False
rev.Reversed = False
doc.recompute()

print(f"Volume: {rev.Shape.Volume:.4f} mm³")
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The profile sketch. Inherited from `ProfileBased`. |
| `Type` | `App::PropertyEnumeration` | `"Angle"` | Revolution method. Values: `"Angle"`, `"UpToLast"`, `"UpToFirst"`, `"UpToFace"`, `"TwoAngles"`. |
| `ReferenceAxis` | `App::PropertyLinkSub` | — | The axis of revolution. When set, `Base` and `Axis` are derived from it automatically. |
| `Angle` | `App::PropertyAngle` | `360.0` | Sweep angle in degrees. Range: [0, 360]. |
| `Angle2` | `App::PropertyAngle` | `0.0` | Second sweep angle for `"TwoAngles"` mode. Range: [0, 360]. |
| `Midplane` | `App::PropertyBool` | `False` | Extend symmetrically on both sides of the sketch plane. Inherited from `ProfileBased`. |
| `Reversed` | `App::PropertyBool` | `False` | Reverse the direction of revolution. Inherited from `ProfileBased`. |
| `UpToFace` | `App::PropertyLinkSub` | — | Target face for `"UpToFace"` mode. |
| `Base` | `App::PropertyVector` | `(0,0,0)` | Base point of the revolution axis. Read-only; derived from `ReferenceAxis`. |
| `Axis` | `App::PropertyVector` | `(0,1,0)` | Direction of the revolution axis. Read-only; derived from `ReferenceAxis`. |

---

## See also

- [Groove](groove.md) — identical parameters, removes material instead of adding it
- [Pad](pad.md) — straight extrusion for non-rotational profiles
- [Additive Pipe](additive-pipe.md) — sweeps a profile along an arbitrary curve
- [Additive Helix](additive-helix.md) — sweeps a profile along a helical path
- [Additive Loft](additive-loft.md) — blends through multiple profiles without a fixed axis
