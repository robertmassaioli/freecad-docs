# Additive Pipe

> **In one sentence:** Sweep a closed 2-D profile sketch along a path curve to
> create a solid whose cross-section follows that path.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Features → Additive Pipe  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Additive Pipe takes two inputs — a *profile* sketch and a *path* (spine) — and
produces a solid by dragging the profile along the path from one end to the
other. The result is fused into the active Body, adding material. Unlike Pad,
which only moves in a straight line, Additive Pipe can follow any curve, making
it essential for tubular shapes, handrails, tracks, wiring channels, and anything
else whose cross-section travels a non-straight route.

---

## Intuition

Think of pasta making: you press dough through a die (the *profile*) while moving
the die along a curved guide (the *path*). Whatever shape the die has, the pasta
takes that shape all the way along the guide's length.

The profile sketch defines the *shape* of the cross-section. The spine defines
the *route* it travels. The two are independent: a circular profile on a straight
spine gives you a cylinder; the same circular profile on a helical spine gives you
a coil spring.

One non-obvious detail: the profile is attached at one end of the spine at the
start of the operation and moves toward the other end. The profile sketch does
not need to be drawn *on* the spine — FreeCAD positions it correctly as long as
the profile's origin lies on or near the spine's start point.

---

## When to use it

- You need a tube, pipe, channel, or conduit that follows a 2-D or 3-D curve.
- You are modelling a handrail, cable duct, wiring loom, or any shape that has
  a consistent cross-section along a non-straight path.
- You want to model a coil spring (combine a circular profile with a helical spine
  sketch, or use [Additive Helix](additive-helix.md) for the specific spring case).
- You have a complex path made of multiple joined edges and need a single sweep
  operation that treats them as one continuous route.
- You need the cross-section to *morph* between two different profiles along the
  path (use Multisection transform mode — see [Parameters](#parameters)).

---

## When NOT to use it

- **The cross-section stays constant and the path is a straight line** — use
  [Pad](pad.md) instead; it is simpler to set up and less prone to failure.
- **The cross-section revolves around a central axis** — use
  [Revolution](revolution.md); it is the right tool for axisymmetric solids.
- **You need the solid to blend smoothly through several free-form profiles in
  3-D space** — use [Additive Loft](additive-loft.md), which does not require a
  spine sketch.
- **The path is a helix with a defined pitch and height** — use
  [Additive Helix](additive-helix.md), which generates the helical path for you.
- **You want to remove material** — use [Subtractive Pipe](subtractive-pipe.md),
  which has identical parameters but cuts instead of adds.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one existing sketch. You will
need two sketches: a *profile* (cross-section shape) and a *spine* (the path).
Both must be inside the Body before you start.

1. **Create the profile sketch.** Draw and fully constrain the cross-section
   shape on the XY plane or any datum plane. The profile must be a single closed
   wire (or multiple nested closed wires for a hollow section). Close the
   Sketcher task panel.

2. **Create the spine sketch.** Draw the path curve on a *different* plane (for
   a flat path, the XZ plane is common). The spine can be an open or closed wire.
   A single continuous, connected wire is required — disconnected edges will cause
   a failure. Close the Sketcher task panel.

3. **Activate Part Design → Additive Features → Additive Pipe.** The task panel
   opens with three collapsible sections: *Pipe Parameters*, *Section
   Orientation*, and *Section Transformation*.

4. **Set the Profile.** In *Pipe Parameters*, click **Object** next to the
   Profile field, then click the profile sketch in the model tree or viewport.
   The sketch name appears in the text field.

5. **Set the Path.** Under *Path to Sweep Along*, click **Object**, then click
   the spine sketch. Alternatively, click **Add edge** to pick individual edges
   from any geometry in the Body instead of using a whole sketch.

6. **Set Corner transition** if the spine has sharp corners:

    - **Transformed** (default) — the profile rotates to match the tangent at
      each corner; may produce self-intersecting geometry at tight bends.
    - **Right corner** — the pipe makes a mitre-cut join at each corner.
    - **Round corner** — the pipe is rounded at each corner.

7. **Adjust Orientation mode** in *Section Orientation* if needed (see
   [Parameters](#parameters)). Leave it on **Standard** for most pipes.

8. **Click OK.** FreeCAD sweeps the profile along the path and fuses the result
   into the Body.

!!! tip
    If the spine is a sketch, you can select the entire sketch as the path
    (click its entry in the model tree). If the spine is made of edges on an
    existing solid face, use **Add edge** to select them individually.

---

## Parameters

The task panel is divided into three sections.

### Pipe Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Profile | Object link | — | The sketch (or face/wire sub-element) whose outline is swept. Must be a closed profile for an additive solid. |
| Corner transition | Dropdown | Transformed | How the sweep behaves at kinks in the spine: `Transformed` (shear), `Right corner` (mitre), `Round corner` (fillet). See deep dive below. |
| Path to Sweep Along | Object link + edge list | — | The spine object (typically a sketch) or a list of individual edges that form the path. Edges must form one connected, non-branching wire. |
| Add edge / Remove edge | Buttons | — | Toggle selection mode to add or remove individual edges from the edge list when you want a partial path from an existing shape. |

### Section Orientation

Controls how the profile is rotated as it travels along the path.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Orientation mode | Dropdown | Standard | How the profile rotates as it travels the path: Standard / Fixed / Frenet / Auxiliary / Binormal. See deep dive below. |
| Curvilinear equivalence | Checkbox | On | (Auxiliary mode only) Distribute orientation points by arc length rather than parallel projection. |
| Auxiliary spine | Object link + edge list | — | (Auxiliary mode only) A secondary path that drives the profile's roll angle. |
| X / Y / Z | Float | 0.0 | (Binormal mode only) The fixed binormal vector components. |

--8<-- "part-design/pipe-orientation-mode.md"

--8<-- "part-design/pipe-corner-transition.md"

### Section Transformation

Controls whether the profile changes shape along the path.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Transform mode | Dropdown | Constant | `Constant`: identical profile at every point. `Multisection`: profile morphs through intermediate section sketches. See deep dive below. |
| Add Section / Remove Section | Buttons | — | (Multisection only) Pick additional profile sketches. |
| Section list | Ordered list | — | (Multisection only) Intermediate sections in path order, top = spine start. Reorder by drag-and-drop. |

--8<-- "part-design/pipe-transform-mode.md"

---

## Advanced usage

### Multisection sweeps

In Multisection mode the sweep morphs between the start profile and each
intermediate section in order. This is the right approach for a shape like a
nozzle that transitions from a round inlet to a rectangular outlet:

1. Draw the start profile (e.g. circle) attached to the spine's start.
2. Draw intermediate section sketches positioned at desired points along the spine
   (attach each to a datum plane perpendicular to the spine at that point).
3. In *Section Transformation*, switch to **Multisection** and add the
   intermediate sketches in path order.

All sections must have the same number of closed loops (wires). The last section
can be a single point (which closes the solid to a cone tip or similar).

### Spine from existing geometry

The spine does not have to be a dedicated sketch. You can click **Add edge** and
select edges directly from a face of an existing solid in the Body. This is useful
for routing a cable channel along the edge of an enclosure.

### Open vs. closed spines

If the spine is a closed loop (e.g. a circle), the resulting solid is a torus-like
shape — the profile travels all the way around and meets itself. Ensure the
profile's origin lies on the spine; otherwise the solid will self-intersect.

---

## Common mistakes and pitfalls

!!! warning "Spine is not connected"
    **Cause:** The selected edges or spine sketch contain a gap — the path is two
    or more disconnected segments.  
    **Fix:** Edit the spine sketch to ensure every line or arc shares endpoints with
    its neighbours, or use the **Add edge** workflow to select only a contiguous
    sequence of edges.

!!! warning "Pipe could not be built / Result is not a solid"
    **Cause:** The profile is too large for the curvature of the path at some
    point. If the cross-section overhangs the centre of curvature, OCCT cannot
    form a valid solid.  
    **Fix:** Reduce the profile size, increase the path's bend radius, or switch
    the Corner transition to **Round corner** to smooth sharp turns.

!!! warning "Profile sketch is not closed"
    **Cause:** The profile sketch has an open wire (a line that does not connect
    back to the start), which cannot form a solid face.  
    **Fix:** Open the profile sketch, close the gap (add a closing line or enable
    *Close shape* for curves), and ensure the Sketcher reports zero open edges.

!!! warning "Unexpected profile twist along the path"
    **Cause:** The **Standard** orientation mode can produce a visible twist on
    certain curved paths, especially when the path has low curvature (nearly
    straight sections).  
    **Fix:** Try **Frenet** mode, or use **Auxiliary** mode with a second guiding
    spine to take explicit control of the roll angle.

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

# Profile sketch: a circle of radius 5 mm on the XY plane
profile = doc.addObject("Sketcher::SketchObject", "Profile")
body.addObject(profile)
profile.addGeometry(Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 5), False)
profile.addConstraint(Sketcher.Constraint("Coincident", 0, 3, -1, 1))
doc.recompute()

# Spine sketch: a line 50 mm along Y, on the XZ plane
spine = doc.addObject("Sketcher::SketchObject", "Spine")
body.addObject(spine)
spine.MapMode = "FlatFace"
spine.AttachmentSupport = (doc.XZ_Plane, [""])
doc.recompute()
spine.addGeometry(Part.LineSegment(App.Vector(0, 0, 0), App.Vector(0, 50, 0)), False)
spine.addConstraint(Sketcher.Constraint("Coincident", 0, 1, -1, 1))
doc.recompute()

# Create the Additive Pipe
pipe = doc.addObject("PartDesign::AdditivePipe", "AdditivePipe")
body.addObject(pipe)
pipe.Profile = profile
pipe.Spine = spine
doc.recompute()

print(f"Volume: {pipe.Shape.Volume:.4f} mm³")
# Expected: π × 5² × 50 ≈ 3926.9908 mm³
```

### Key properties

All properties live in the `Sweep` property group.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The profile sketch or sub-element. Inherited from `ProfileBased`. |
| `Spine` | `App::PropertyLinkSub` | — | The path object and optionally specific sub-edges. |
| `SpineTangent` | `App::PropertyBool` | `False` | When `True`, tangent-continuous edges adjoining the spine are automatically included in the path. |
| `AuxiliarySpine` | `App::PropertyLinkSub` | — | Secondary spine used when `Mode` is `"Auxiliary"`. |
| `AuxiliarySpineTangent` | `App::PropertyBool` | `False` | Include tangent-continuous edges into the auxiliary spine. |
| `AuxiliaryCurvilinear` | `App::PropertyBool` | `True` | Use curvilinear equivalence when computing orientation from the auxiliary spine. |
| `Mode` | `App::PropertyEnumeration` | `"Standard"` | Profile orientation mode. Values: `"Standard"`, `"Fixed"`, `"Frenet"`, `"Auxiliary"`, `"Binormal"`. |
| `Binormal` | `App::PropertyVector` | `(0, 0, 0)` | Fixed binormal vector, used only when `Mode` is `"Binormal"`. |
| `Transition` | `App::PropertyEnumeration` | `"Transformed"` | Corner behaviour. Values: `"Transformed"`, `"Right corner"`, `"Round corner"`. |
| `Transformation` | `App::PropertyEnumeration` | `"Constant"` | Section transform mode. Values: `"Constant"`, `"Multisection"`. |
| `Sections` | `App::PropertyLinkSubList` | `[]` | Intermediate section profiles for Multisection mode, in path order. |

---

## See also

- [Subtractive Pipe](subtractive-pipe.md) — identical parameters, removes material instead
- [Additive Loft](additive-loft.md) — blends through multiple profiles without a spine
- [Additive Helix](additive-helix.md) — sweeps a profile along a helix; best for springs and threads
- [Pad](pad.md) — simpler extrusion for straight paths
- [Revolution](revolution.md) — simpler rotation for axisymmetric profiles
