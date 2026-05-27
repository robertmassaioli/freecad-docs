# Subtractive Pipe

> **In one sentence:** Sweep a closed 2-D profile sketch along a path curve to cut a channel or
> groove whose cross-section follows that path.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Features → Subtractive Pipe  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Subtractive Pipe takes the same two inputs as [Additive Pipe](additive-pipe.md) — a *profile*
sketch and a *path* (spine) — but subtracts the resulting swept solid from the active Body instead
of adding to it. The result is a channel, groove, or hollow tunnel whose cross-section follows the
chosen path from one end to the other. All parameters are identical to Additive Pipe; only the
Boolean operation (cut vs. fuse) differs.

---

## Intuition

Think of a router bit moving along a curved guide. The bit has a specific shape (the *profile*),
and the guide controls where the bit travels (the *path*). Whatever shape the bit has, the groove
left in the workpiece mirrors that shape all the way along the guide's length.

The profile sketch defines the *shape* of the cross-section being removed. The spine defines the
*route* it travels. The two are independent: a circular profile on a straight spine carves a
cylindrical bore; the same circular profile on a curved spine carves a bent tunnel.

Because the profile is dragged along the spine starting at one end, the sketch does not need to be
drawn *on* the spine. FreeCAD positions it correctly as long as the profile's origin lies on or
near the spine's start point.

---

## When to use it

- You need to cut a channel, conduit, or wiring duct that follows a curved route through a part.
- You are machining a curved slot or groove whose cross-section must remain constant along its
  length.
- You want to model a fluid passage, pipe recess, or cable raceway inside a housing.
- You need the cut to morph between two cross-section shapes along the path (use Multisection
  transform mode — see [Parameters](#parameters)).
- The path is assembled from multiple joined edges, and you want a single operation to cut along
  all of them as one continuous route.

---

## When NOT to use it

- **The cut has a constant cross-section and a straight path** — use [Pocket](pocket.md); it is
  simpler and less prone to failure.
- **The cut revolves around a central axis** — use [Groove](groove.md), which handles axisymmetric
  cuts.
- **The profiles at the start and end are different and there is no explicit spine** — use
  [Subtractive Loft](subtractive-loft.md), which blends between sections without requiring a spine
  sketch.
- **The path is a helix** — use [Subtractive Helix](subtractive-helix.md), which generates the
  helical path automatically.
- **You want to add material** — use [Additive Pipe](additive-pipe.md).

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with at least one existing solid feature to cut into. You will
need two sketches: a *profile* (cross-section shape) and a *spine* (the path). Both must belong to
the Body before you start.

1. **Create the profile sketch.** Draw and fully constrain the cross-section shape on any plane or
   datum plane. The profile must be a single closed wire (or multiple nested closed wires for a
   hollow section). Close the Sketcher task panel.

2. **Create the spine sketch.** Draw the path curve on a *different* plane. The spine can be an
   open or closed wire; it must be one continuous, connected wire — disconnected edges will cause a
   failure. Close the Sketcher task panel.

3. **Activate Part Design → Subtractive Features → Subtractive Pipe.** The task panel opens with
   three collapsible sections: *Pipe Parameters*, *Section Orientation*, and *Section
   Transformation*.

4. **Set the Profile.** In *Pipe Parameters*, click **Object** next to the Profile field, then
   click the profile sketch in the model tree or viewport.

5. **Set the Path.** Under *Path to Sweep Along*, click **Object**, then click the spine sketch.
   Alternatively, click **Add edge** to pick individual edges from any geometry in the Body instead
   of using a whole sketch.

6. **Set Corner transition** if the spine has sharp corners:

    - **Transformed** (default) — the profile rotates to match the tangent at each corner; may
      produce self-intersecting geometry at tight bends.
    - **Right corner** — the pipe makes a mitre-cut join at each corner.
    - **Round corner** — the pipe is rounded at each corner.

7. **Adjust Orientation mode** in *Section Orientation* if needed (see [Parameters](#parameters)).
   Leave it on **Standard** for most channels.

8. **Click OK.** FreeCAD sweeps the profile along the path and subtracts the result from the Body.

!!! tip
    If the spine is a sketch, select the entire sketch in the model tree as the path. If the spine
    is made of edges on an existing solid face, use **Add edge** to select them individually.

---

## Parameters

The task panel is divided into three sections. All parameters are identical to
[Additive Pipe](additive-pipe.md).

### Pipe Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Profile | Object link | — | The sketch (or face/wire sub-element) whose outline is swept. Must be a closed profile to form a solid cut. |
| Corner transition | Dropdown | Transformed | How the sweep behaves at kinks in the spine: `Transformed` (shear), `Right corner` (mitre), `Round corner` (fillet). See deep dive below. |
| Path to Sweep Along | Object link + edge list | — | The spine object (typically a sketch) or a list of individual edges that form the path. Edges must form one connected, non-branching wire. |
| Add edge / Remove edge | Buttons | — | Toggle selection mode to add or remove individual edges from the edge list. |

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

Controls whether the cut profile changes shape along the path.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Transform mode | Dropdown | Constant | `Constant`: identical cut profile at every point. `Multisection`: cut profile morphs through intermediate section sketches. See deep dive below. |
| Add Section / Remove Section | Buttons | — | (Multisection only) Pick additional profile sketches. |
| Section list | Ordered list | — | (Multisection only) Intermediate sections in path order, top = spine start. |

--8<-- "part-design/pipe-transform-mode.md"

---

## Advanced usage

### Multisection sweeps

In Multisection mode the cut morphs between the start profile and each intermediate section in
order. This lets you carve a recess that transitions from a round entry to a rectangular channel:

1. Draw the start profile (e.g. a circle) positioned at the spine's start.
2. Draw intermediate section sketches at desired points along the spine, each attached to a datum
   plane perpendicular to the spine at that point.
3. In *Section Transformation*, switch to **Multisection** and add the intermediate sketches in
   path order.

All sections must have the same number of closed loops as the start profile. The last section can
be a single point to produce a tapered tip.

### Spine from existing geometry

The spine does not have to be a dedicated sketch. Click **Add edge** and select edges directly
from a face of an existing solid in the Body. This is useful for cutting a groove along the edge
profile of an enclosure wall.

### Cutting with a closed spine

If the spine forms a closed loop (e.g. a circle), the swept cut wraps around and meets itself,
producing a torus-shaped cavity. Make sure the profile origin lies on the spine to avoid
self-intersecting geometry.

---

## Common mistakes and pitfalls

!!! warning "Spine is not connected"
    **Cause:** The selected edges or spine sketch contain a gap — the path is two or more
    disconnected segments.  
    **Fix:** Edit the spine sketch to ensure every line or arc shares endpoints with its
    neighbours, or use **Add edge** to select only a contiguous sequence of edges.

!!! warning "Pipe could not be built / Result is not a solid"
    **Cause:** The profile is too large for the curvature of the path at some point. If the
    cross-section overhangs the centre of curvature, OCCT cannot form a valid solid to subtract.  
    **Fix:** Reduce the profile size, increase the path's bend radius, or switch the Corner
    transition to **Round corner** to smooth sharp turns.

!!! warning "Profile sketch is not closed"
    **Cause:** The profile sketch has an open wire, which cannot form a solid face to sweep.  
    **Fix:** Open the profile sketch, close the gap, and ensure the Sketcher reports zero open
    edges.

!!! warning "Unexpected profile twist along the path"
    **Cause:** The **Standard** orientation mode can produce a visible twist on certain curved
    paths, especially when the path has sections of low curvature.  
    **Fix:** Try **Frenet** mode, or use **Auxiliary** mode with a second guiding spine to take
    explicit control of the roll angle.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create the body with a solid block to cut into
body = doc.addObject("PartDesign::Body", "Body")

base_sk = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sk)
base_sk.addGeometry(
    Part.makePolygon([
        App.Vector(-25, -25, 0), App.Vector(25, -25, 0),
        App.Vector(25, 25, 0), App.Vector(-25, 25, 0),
        App.Vector(-25, -25, 0),
    ])
)
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = base_sk
pad.Length = 60.0
doc.recompute()

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

# Create the Subtractive Pipe
pipe = doc.addObject("PartDesign::SubtractivePipe", "SubtractivePipe")
body.addObject(pipe)
pipe.Profile = profile
pipe.Spine = spine
doc.recompute()
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
| `Transformation` | `App::PropertyEnumeration` | `"Constant"` | Section transform mode. Values: `"Constant"`, `"Multisection"`, `"Linear"`, `"S-shape"`, `"Interpolation"`. |
| `Sections` | `App::PropertyLinkSubList` | `[]` | Intermediate section profiles for Multisection mode, in path order. |

---

## See also

- [Additive Pipe](additive-pipe.md) — identical parameters, adds material instead of removing it
- [Subtractive Loft](subtractive-loft.md) — cuts through multiple profiles without a spine
- [Subtractive Helix](subtractive-helix.md) — cuts a swept solid along a helix path
- [Pocket](pocket.md) — simpler straight cut for constant cross-sections
- [Groove](groove.md) — axisymmetric cut around a central axis
