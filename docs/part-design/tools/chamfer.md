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
| Type | Dropdown | `Equal distance` | The geometric mode. Options: `Equal distance`, `Two distances`, `Distance and Angle`. Controls which of the fields below are active. |
| Size | Length | `1 mm` | The setback distance from the edge on the first (or only) side. Must be greater than zero. |
| Size2 | Length | `1 mm` | The setback distance on the second side. Active only in *Two distances* mode. Must be greater than zero. |
| Angle | Angle | `45°` | The inclination of the chamfer face. Active only in *Distance and Angle* mode. Valid range: `> 0°` and `< 180°`. |
| Flip direction | Toggle | off | Swaps which adjacent face receives *Size* and which receives *Size2* or is used as the angle reference. Disabled in *Equal distance* mode. |
| Use all edges | Toggle | off | Chamfers every edge of the base solid; the reference list is ignored when enabled. |
| References (edge list) | Sub-element links | — | The edges or faces whose edges will be chamfered. Active only when *Use all edges* is off. |

---

## Advanced usage

### Chamfer type mode details

In **Equal distance** mode, OCCT uses the same inset distance on both sides,
producing a symmetric bevel. The chamfer face is coplanar through the midpoint
of the two faces' angular bisector.

In **Two distances** mode, *Size* is the setback on the face that the viewport
tooltip labels "first face" (typically the face with the lower topological index
when selected), and *Size2* is the setback on the other face. If the result looks
swapped, use **Flip direction** to correct it without changing the distance values.

In **Distance and Angle** mode, *Size* sets how far back from the edge the
chamfer starts on the first face, and *Angle* is measured from that face to the
chamfer surface.

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
