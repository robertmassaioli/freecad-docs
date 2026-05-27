# Chamfer

> **In one sentence:** Cut a flat angled bevel along selected edges of a solid,
> replacing each sharp corner with a planar face.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Dress-Up Features → Chamfer  
**Toolbar:** Part Design Dress-Up Features · **Shortcut:** none

Chamfer replaces one or more sharp edges of the active Body's solid with a flat
angled face. Three geometric modes are available: equal distance from both
adjacent faces, two independent distances, or a distance combined with an angle.
Like all dress-up features, Chamfer is fully parametric — changing any dimension
updates the solid immediately.

---

## Intuition

Imagine filing the corner of a metal bracket at 45 degrees. The sharp edge is
replaced by a small flat face set at an angle to both original surfaces. That is
an equal-distance chamfer: both adjacent faces are trimmed by the same amount.

In contrast, *Two distances* mode lets you set different setbacks on each side,
creating an asymmetric bevel. *Distance and Angle* mode lets you control one
setback distance and the angle of the chamfer face, which is equivalent to
specifying how far back from the edge the bevel starts and how steep it is.

The difference between Chamfer and [Fillet](fillet.md) is simple: Fillet gives a
smooth curved surface; Chamfer gives a flat face. Chamfers are common in machined
metal parts (to break sharp edges and allow assembly), while fillets are more
common in cast or moulded parts (for strength and mould release).

---

## When to use it

- You need to break a sharp edge on a machined part for safety or assembly
  reasons (a 45° chamfer at 0.5 mm is a very common engineering callout).
- You are modelling a countersunk screw hole and want the conical lead-in.
- You need an asymmetric bevel where different amounts of material are removed
  from each side of an edge.
- You want to combine a distance with an angle — for example, a chamfer that
  starts 2 mm back from the edge and is inclined at 60°.
- You need to add a chamfer to every edge at once (use *Use All Edges*).

---

## When NOT to use it

- **You want a smooth curved blend rather than a flat cut** — use
  [Fillet](fillet.md) instead.
- **The edge is on a sketch profile** — add a chamfer geometry directly in
  Sketcher before padding it.
- **You are working on a Part workbench solid outside a Body** — use
  `Part::Chamfer` from the Part workbench.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body with at least one solid feature containing edges.

1. **Select the edges to chamfer.** Click one or more edges in the 3-D viewport.
   Hold ++ctrl++ to add edges to the selection. You may also select a face to
   pick all its bounding edges. Pre-selection is optional — you can add edges
   from the task panel.

2. **Open Chamfer.** Go to **Part Design → Dress-Up Features → Chamfer**, or
   click the Chamfer button in the *Part Design Dress-Up Features* toolbar.

3. **Choose the chamfer type.** In the task panel, select one of:
    - **Equal distance** — one distance value; both sides trimmed equally.
    - **Two distances** — separate *Size* (first side) and *Size2* (second side).
    - **Distance and Angle** — *Size* controls the setback on the first side;
      *Angle* controls the inclination of the chamfer face.

4. **Set the size.** Type a value in the *Size* field. Default is `1 mm`.

5. **Set Size2 or Angle** if you chose *Two distances* or *Distance and Angle*.

6. **Flip direction** if needed. For *Two distances* and *Distance and Angle*
   modes, tick *Flip direction* to swap which adjacent face receives *Size*
   versus *Size2* (or which side the angle is measured from).

7. **Click OK.** The Chamfer feature is added to the model tree.

!!! tip
    Enable *Use all edges* to apply the same chamfer to every edge on the solid
    at once, without selecting individual edges.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Type | Dropdown | `Equal distance` | The geometric mode: `Equal distance`, `Two distances`, or `Distance and Angle`. See deep dive below. |
| Size | Length | `1 mm` | Setback distance on the first (or only) side. Must be > 0. |
| Size2 | Length | `1 mm` | Setback distance on the second side. Active only in *Two distances* mode. Must be > 0. |
| Angle | Angle | `45°` | Inclination of the chamfer face. Active only in *Distance and Angle* mode. Range: > 0° and < 180°. |
| Flip direction | Toggle | off | Swaps which face receives *Size* vs *Size2* (or the angle reference). Disabled in *Equal distance* mode. |
| Use all edges | Toggle | off | Chamfers every edge on the solid; the reference list is ignored. |
| References (edge list) | Sub-element links | — | Edges to chamfer. Active only when *Use all edges* is off. |

### Type in depth

The Type dropdown controls the geometry of the chamfer face — specifically,
how the two measurements that define the bevel are specified.

#### Equal distance (default)

Both sides of the chamfer set back by the same distance (*Size*). The chamfer
face bisects the angle between the two adjacent faces symmetrically.

Intuition: imagine sanding a 45° bevel where both faces are touched equally —
regardless of what angle the original edge subtends, each adjacent face loses
the same amount of material.

**Use this mode when:**
- You want a clean symmetric bevel and the setback is the same on both sides
- Dimensioning by a single value (callout: "chamfer 1×45°" in machining)
- The two adjacent faces meet at 90° (the most common case)

**Watch out for:** On edges where the two faces meet at an angle other than 90°,
"equal distance" still means equal *setback*, not a 45° angle face. The chamfer
face angle will vary depending on the dihedral angle of the edge.

#### Two distances

Each side of the chamfer has an independent setback: *Size* on the first face
and *Size2* on the second face. The chamfer face is a plane that connects the
two setback lines.

Intuition: one side retreats 2 mm and the other retreats 0.5 mm — like a
ramp that is steep on one side and shallow on the other.

**Use this mode when:**
- The chamfer must be asymmetric (e.g. more clearance on one side than the other)
- Matching a specific engineering callout (e.g. "chamfer 2 × 0.5")
- The edge connects faces of different functional importance and each needs its
  own setback

Sub-parameters active: **Size2**, **Flip direction**

**Watch out for:** FreeCAD assigns "first face" and "second face" based on
topological face index, which is not always predictable from the viewport. If
the result looks swapped, click **Flip direction** — this is the correct fix,
not changing which edge you selected.

#### Distance and angle

One side is set back by *Size* and the chamfer face is inclined at *Angle* from
that face. The second side's setback is derived from the geometry.

Intuition: lay a ruler flat on one face, measure *Size* in from the edge, then
tilt a blade up from that point at *Angle* degrees — the tilted blade face is
the chamfer.

**Use this mode when:**
- The chamfer is specified by one distance and one angle (common in machining
  drawings: "chamfer 1 mm × 30°")
- The chamfer needs to reach a specific depth on one face while the angle is the
  primary design constraint
- You are modelling a chamfer that guides a fastener into a hole (60°–90°
  entry chamfers)

Sub-parameters active: **Angle**, **Flip direction**

**Watch out for:** The angle is measured from the *first face* to the chamfer
surface — not from the edge bisector. If the angle is applied from the wrong
face, the result will look correct in angle but incorrect in which face is
being measured from. Use **Flip direction** to change the reference face.

---

## Advanced usage

### Migration from FreeCAD 0.21

Files created in FreeCAD 0.21 with *Two distances* or *Distance and Angle*
chamfers will have their `FlipDirection` property automatically adjusted when
opened in 1.x because the interpretation of *Size* and *Size2* was reversed.
Re-saving in 1.x and re-opening in 0.21 will again produce a different result —
this is a known one-way migration.

---

## Common mistakes and pitfalls

!!! warning "Size must be greater than zero"
    **Cause:** A size value of zero or negative was entered.  
    **Fix:** Enter a positive value in the *Size* (and *Size2* if applicable)
    field.

!!! warning "Angle must be greater than 0 and less than 180"
    **Cause:** In *Distance and Angle* mode, the angle is outside the valid range.  
    **Fix:** Enter an angle strictly between `0°` and `180°`. Typical values are
    between `15°` and `75°`; `45°` is the most common.

!!! warning "Chamfer result looks correct in one mode but wrong after flipping"
    **Cause:** The *Flip direction* checkbox swaps which face is the reference for
    *Size*. In asymmetric modes the visual result changes completely.  
    **Fix:** Think of *Size* as "how far back on the face I first clicked" and
    *Size2* as "how far back on the other face." Use *Flip direction* if the
    assignment is reversed.

!!! warning "Failed to create chamfer"
    **Cause:** The geometry at the selected edge(s) is incompatible with the
    chosen chamfer size — often because the size is larger than the adjacent face
    or the edge is part of a complex curve.  
    **Fix:** Reduce the size, select fewer edges, or chamfer edges individually
    to isolate the problem edge.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign
import Sketcher
import Part

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
body.addObject(sketch)
sketch.addGeometry(Part.LineSegment(App.Vector(0,0,0), App.Vector(20,0,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(20,0,0), App.Vector(20,20,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(20,20,0), App.Vector(0,20,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(0,20,0), App.Vector(0,0,0)), False)
sketch.addConstraint(Sketcher.Constraint("Coincident", 0,2, 1,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 1,2, 2,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 2,2, 3,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 3,2, 0,1))
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = sketch
pad.Length = 15.0
doc.recompute()

# Equal-distance chamfer on all edges, 1 mm
chamfer = doc.addObject("PartDesign::Chamfer", "Chamfer")
body.addObject(chamfer)
chamfer.Base = (pad, [])         # TODO: verify edge sub-element selection syntax
chamfer.ChamferType = 0          # 0 = "Equal distance"
chamfer.Size = 1.0               # 1 mm setback on both sides
chamfer.UseAllEdges = True
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `ChamferType` | `App::PropertyEnumeration` | `0` = `"Equal distance"`, `1` = `"Two distances"`, `2` = `"Distance and Angle"`. Default: `0`. |
| `Size` | `App::PropertyQuantityConstraint` | First (or only) setback distance. Minimum: `0`, step: `0.1`. Default: `1 mm`. |
| `Size2` | `App::PropertyQuantityConstraint` | Second setback distance; used only when `ChamferType` is `1`. Default: `1 mm`. |
| `Angle` | `App::PropertyAngle` | Chamfer face inclination; used only when `ChamferType` is `2`. Valid range: `0°`–`180°` exclusive. Default: `45°`. |
| `FlipDirection` | `App::PropertyBool` | Swaps the reference faces for asymmetric types. Default: `False`. |
| `UseAllEdges` | `App::PropertyBool` | Apply to every edge of the base shape. Default: `False`. |
| `Base` | `App::PropertyLinkSub` | Link to the base feature and selected sub-edges/faces. Inherited from `DressUp`. |

---

## See also

- [Fillet](fillet.md) — curved blend instead of a flat chamfer
- [Draft](draft.md) — taper faces for mould release
- [Thickness](thickness.md) — hollow out a solid to a uniform wall
- [Pad](pad.md) — the feature most commonly chamfered after creation
