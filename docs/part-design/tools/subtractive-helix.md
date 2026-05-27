# Subtractive Helix

> **In one sentence:** Sweep a profile sketch along a generated helix path to cut a helical groove,
> thread, or coil-shaped void into an existing Body.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Features → Subtractive Helix  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Subtractive Helix sweeps a profile sketch along an internally generated helical path and subtracts
the result from the active Body. Unlike [Subtractive Pipe](subtractive-pipe.md), you do not need to
draw the path — you specify the helix geometry (pitch, height, turns, angle) through the task panel
and FreeCAD builds the path for you. The result is a helical cut such as a screw thread, a worm
gear groove, or a spiral coolant channel. It is the subtractive counterpart to
[Additive Helix](additive-helix.md), which adds material instead of removing it.

One parameter unique to Subtractive Helix is **Outside**: when enabled, the tool keeps only the
intersection of the helical solid with the existing Body, allowing you to trim away the
exterior of a helical shape rather than cut into the interior.

---

## Intuition

Think of a lathe cutting a thread into a rod. The cutting tool (the *profile*) has a specific
tooth shape, and it advances along the rod one pitch per revolution (the *helix parameters*). The
material removed traces a helical groove whose cross-section matches the tool shape exactly.

Subtractive Helix is the digital equivalent. You draw the tooth or groove cross-section as a
profile sketch, then describe the helix — how far it advances per turn (*pitch*), how many turns,
and whether it tapers — and FreeCAD computes the path and performs the cut.

Four *input modes* let you describe the same helix from whichever combination of values you know:
pitch and height, pitch and turns, height and turns, or height and turns with radial growth. Only
the chosen inputs are editable; the remaining values are computed automatically.

---

## When to use it

- You need to cut a screw thread, lead-screw groove, or worm-gear tooth into a cylindrical boss.
- You are machining a helical coolant channel or oil groove into a shaft.
- You want to cut a spiral ramp or helical slot into a cylindrical or conical surface.
- You need an Archimedean spiral groove in a flat disc (use a flat-spiral mode with growth).
- You want to subtract the exterior of a helical solid from a larger body (use **Outside** mode).

---

## When NOT to use it

- **The path is a custom 3-D curve, not a helix** — use [Subtractive Pipe](subtractive-pipe.md)
  with an explicit spine sketch.
- **You need a thread form automatically sized to a standard** — consider the **Hole** tool, which
  has built-in thread profiles for standard fasteners.
- **You want to add material in a helix** — use [Additive Helix](additive-helix.md).
- **The profile must morph shape along the path** — use [Subtractive Pipe](subtractive-pipe.md) in
  Multisection mode.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one existing solid feature to cut into. You need
one profile sketch. The helix axis is derived from the sketch's reference axis (set in the task
panel), so the sketch plane determines the starting orientation.

1. **Create the profile sketch.** Draw and fully constrain the cross-section of the groove or
   thread tooth on any plane. The sketch must produce a closed face. Close the Sketcher task panel.

2. **Activate Part Design → Subtractive Features → Subtractive Helix.** The task panel opens.
   FreeCAD proposes initial pitch and height values based on the profile's bounding box to avoid
   self-intersection.

3. **Set the Axis.** In the *Axis* dropdown, choose the axis around which the helix revolves. You
   can select the sketch's vertical axis, horizontal axis, normal axis, or any linear edge or datum
   line in the Body. The **Base** and **Axis** properties update automatically.

4. **Choose the Input mode.** Select the combination of parameters you want to control directly:

    - **Pitch-Height-Angle** — specify pitch and height; turns and growth are computed.
    - **Pitch-Turns-Angle** — specify pitch and turns; height and growth are computed.
    - **Height-Turns-Angle** — specify height and turns; pitch and growth are computed.
    - **Height-Turns-Growth** — specify height, turns, and radial growth per turn; pitch and angle
      are computed.

5. **Set the helix dimensions.** Enter values for the input parameters chosen in the previous step.
   The read-only fields update to show the derived values.

6. **Toggle Left Handed** if the helix should wind counter-clockwise when viewed along the axis
   (default is right-handed / clockwise).

7. **Toggle Outside** if you want to intersect the helix solid with the Body (retaining only the
   overlap) rather than subtracting the helix from the Body. This is hidden when using Additive
   Helix and only appears for Subtractive Helix.

8. **Click OK.** FreeCAD generates the helix path, sweeps the profile along it, and subtracts the
   result from the Body.

!!! tip
    If FreeCAD reports a self-intersection error, increase the pitch or reduce the profile size.
    The initial auto-proposed pitch is calculated from the profile bounding box and is usually safe;
    if you reduced it manually, increase it back toward the proposed value.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Axis | Reference link (dropdown) | Sketch normal axis | The axis around which the helix revolves. Can be the sketch's own axes, or any linear edge or datum line in the Body. |
| Mode | Dropdown | `pitch-height-angle` | Selects which three parameters are the primary inputs. Values: `pitch-height-angle`, `pitch-turns-angle`, `height-turns-angle`, `height-turns-growth`. |
| Pitch | Length | 10.0 mm | Axial distance between two consecutive turns. Editable only when Mode includes *pitch* as a primary input. |
| Height | Length | 30.0 mm | Total axial length of the helix path. Editable only when Mode includes *height* as a primary input. |
| Turns | Float | 3.0 | Number of complete turns. Must be > 0. Editable only when Mode includes *turns* as a primary input. |
| Cone Angle | Angle | 0.0 ° | Half-angle of the cone that the helix wraps around. Zero = cylindrical helix. Positive = radius grows; negative = radius shrinks. Range: −89 ° to 89 °. Editable only when Mode includes *angle* as a primary input. |
| Growth | Distance | 0.0 mm | Radial growth per turn (for `height-turns-growth` mode). Positive = expanding spiral; negative = contracting. Editable only when Mode is `height-turns-growth`. |
| Left Handed | Checkbox | Off | When on, the helix winds counter-clockwise when viewed along the axis (left-hand thread). Default is right-handed. |
| Reversed | Checkbox | Off | When on, the helix extends in the opposite direction of the axis vector. Inherited from `ProfileBased`. |
| Outside | Checkbox | Off | **Subtractive Helix only.** When on, the Boolean operation is *intersection* (Common) rather than *cut*, keeping only the portion of the Body that overlaps the helical solid. |
| Tolerance | Float | 0.1 | Fusion tolerance used when merging the helical solid. Increase if the result does not merge cleanly with the Body. |

**Input modes:**

| Mode | Primary inputs | Derived |
|------|---------------|---------|
| **pitch-height-angle** | Pitch, Height, Cone Angle | Turns, Growth |
| **pitch-turns-angle** | Pitch, Turns, Cone Angle | Height, Growth |
| **height-turns-angle** | Height, Turns, Cone Angle | Pitch, Growth |
| **height-turns-growth** | Height, Turns, Growth | Pitch, Cone Angle |

--8<-- "part-design/helix-mode.md"

---

## Advanced usage

### Conical (tapered) helices

Setting a non-zero **Cone Angle** turns the helix into a conical spiral — the radius changes along
the axis. A positive angle makes the helix flare outward (useful for tapered thread forms); a
negative angle makes it converge (useful for conical worm cuts). The angle range is −89 ° to 89 °;
values near 90 ° would produce a flat spiral.

### Flat Archimedean spirals

To cut a flat spiral groove in a disc (zero height, radially expanding), use
**Height-Turns-Growth** mode, set Height to zero (or a very small value), and set Growth to the
desired radial step per turn. Note that a zero height is valid in this mode and is not an error —
FreeCAD will not warn about it.

### Using Outside mode for helical trimming

The **Outside** checkbox inverts the Boolean operation from a cut to an intersection. Instead of
removing the helical volume from the Body, FreeCAD retains only the material that is inside both
the Body and the helical volume. This is rarely needed but is essential for operations like cutting
the exterior profile of a helical gear tooth down to size.

### Thread modelling considerations

For cosmetic threads or threads that must match a standard, consider whether Subtractive Helix is
the right tool. The **Hole** tool has built-in ISO and UNC/UNF thread options and handles thread
depth, chamfer, and counterbore automatically. Reserve Subtractive Helix for custom or non-standard
thread profiles, worm-gear grooves, and other helical cuts where you need full control of the
profile shape.

---

## Common mistakes and pitfalls

!!! warning "Error: Result is self intersecting"
    **Cause:** The pitch is too small relative to the profile's axial extent — consecutive turns
    overlap each other.  
    **Fix:** Increase the pitch. Use the auto-proposed value as a lower bound; it is calculated from
    the profile bounding box to ensure non-overlapping turns.

!!! warning "Error: There is nothing to subtract"
    **Cause:** The Body has no existing solid when Subtractive Helix is activated. Subtractive
    operations require an existing solid to cut into.  
    **Fix:** Add a Pad, Revolution, or Additive feature to the Body before activating Subtractive
    Helix.

!!! warning "Error: Face must be planar"
    **Cause:** The profile sketch resolves to a non-planar face (e.g. a sketch on a curved surface).
    The helix implementation requires a planar starting face.  
    **Fix:** Ensure the profile sketch is attached to a flat plane or datum plane.

!!! warning "Helix does not merge cleanly / jagged result"
    **Cause:** The helical path approximation has insufficient numerical precision relative to the
    part's overall size, causing the fusion to fail or leave artefacts.  
    **Fix:** Increase the **Tolerance** parameter. The default is 0.1; try values between 1 and 10
    for very large parts, and values approaching 0.01 for very small parts.

!!! warning "Pitch too small error at zero height"
    **Cause:** In `pitch-height-angle` or `pitch-turns-angle` mode, both pitch and height must be
    greater than zero.  
    **Fix:** Use **Height-Turns-Growth** mode for flat spirals where height is zero.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create a cylindrical body to cut into
body = doc.addObject("PartDesign::Body", "Body")

base_sk = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sk)
base_sk.addGeometry(Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 15), False)
base_sk.addConstraint(Sketcher.Constraint("Coincident", 0, 3, -1, 1))
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = base_sk
pad.Length = 40.0
doc.recompute()

# Profile sketch: a triangle tooth for the thread groove, on the XZ plane
profile = doc.addObject("Sketcher::SketchObject", "Profile")
body.addObject(profile)
profile.MapMode = "FlatFace"
profile.AttachmentSupport = (doc.XZ_Plane, [""])
doc.recompute()
# Simple triangle profile  # TODO: verify exact sketch geometry setup
profile.addGeometry(
    Part.makePolygon([
        App.Vector(10, 0, 0), App.Vector(12, 0, 1.5), App.Vector(14, 0, 0),
        App.Vector(10, 0, 0),
    ])
)
doc.recompute()

# Create the Subtractive Helix
helix = doc.addObject("PartDesign::SubtractiveHelix", "SubtractiveHelix")
body.addObject(helix)
helix.Profile = profile
helix.Pitch = 5.0     # mm between turns
helix.Height = 30.0   # total axial length
helix.Turns = 6.0     # computed from pitch and height
doc.recompute()

print(f"Helix feature: {helix.Shape.Volume:.2f} mm³ remaining")
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The profile sketch or sub-element. Inherited from `ProfileBased`. |
| `ReferenceAxis` | `App::PropertyLinkSub` | — | The axis around which the helix revolves. When set, `Base` and `Axis` are derived from it automatically. |
| `Base` | `App::PropertyVector` | `(0, 0, 0)` | Centre point of the helix start (read-only; derived from `ReferenceAxis`). |
| `Axis` | `App::PropertyVector` | `(0, 1, 0)` | Helix direction vector (read-only; derived from `ReferenceAxis`). |
| `Mode` | `App::PropertyEnumeration` | `"pitch-height-angle"` | Input mode. Values: `"pitch-height-angle"`, `"pitch-turns-angle"`, `"height-turns-angle"`, `"height-turns-growth"`. |
| `Pitch` | `App::PropertyLength` | `10.0` | Axial distance per turn (mm). |
| `Height` | `App::PropertyLength` | `30.0` | Total helix axial length (mm). |
| `Turns` | `App::PropertyFloatConstraint` | `3.0` | Number of turns. Must be > 0. |
| `Angle` | `App::PropertyAngle` | `0.0` | Cone half-angle (degrees). Range: −89 to 89. |
| `Growth` | `App::PropertyDistance` | `0.0` | Radial growth per turn (mm), for `height-turns-growth` mode. |
| `LeftHanded` | `App::PropertyBool` | `False` | Counter-clockwise winding when viewed along axis. |
| `Reversed` | `App::PropertyBool` | `False` | Extend helix in the opposite direction of `Axis`. Inherited from `ProfileBased`. |
| `Outside` | `App::PropertyBool` | `False` | Use intersection (Common) instead of Cut. Visible only for SubtractiveHelix. |
| `Tolerance` | `App::PropertyFloatConstraint` | `0.1` | Fusion tolerance multiplier. Increase for large parts or if result is jagged. |

---

## See also

- [Additive Helix](additive-helix.md) — identical parameters, adds material instead of removing it
- [Subtractive Pipe](subtractive-pipe.md) — removes material swept along a custom spine curve
- [Subtractive Loft](subtractive-loft.md) — removes material blended through multiple profiles
- [Pocket](pocket.md) — simpler straight cut for constant cross-sections
- [Groove](groove.md) — axisymmetric cut around a central axis
