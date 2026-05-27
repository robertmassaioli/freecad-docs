# Arcs (Circular, Elliptical, Hyperbolic, Parabolic)

> **In one sentence:** Draw curved arc segments — from simple circular arcs to
> conic section arcs — that can be joined with lines or other curves to form
> complex sketch profiles.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Arc  
**Toolbar:** Sketcher Geometries (dropdown) · **Shortcut:** `G, A` (default: Arc from Center)

The Arc group contains five arc tools grouped under one toolbar dropdown:

| Tool | Shortcut | Description |
|------|----------|-------------|
| Arc From Center | `G, A` | Three clicks: center, start angle, end angle |
| Arc From 3 Points | `G, 3, A` | Three clicks: start, end, and a point on the arc |
| Elliptical Arc | `G, E, A` | Elliptical arc section (arc of an ellipse) |
| Hyperbolic Arc | `G, H` | Arc of a hyperbola |
| Parabolic Arc | `G, J` | Arc of a parabola |

For most work, use **Arc From Center** or **Arc From 3 Points**. The conic arcs
(elliptical, hyperbolic, parabolic) are specialised tools for optical and
mathematical geometry.

---

## Intuition

A circular arc is a portion of a circle — think of trimming a circle to just the
segment you need. Arc From Center specifies *where the centre is and the angular
sweep*; Arc From 3 Points specifies *two endpoints and one intermediate point* and
FreeCAD computes the circle that passes through all three.

The conic arcs follow the same principle but with non-circular curves: an
elliptical arc is a portion of an ellipse, a hyperbolic arc is a portion of one
branch of a hyperbola, and a parabolic arc is a portion of a parabola.

---

## When to use it

- You need a rounded corner or curved section in a profile (use Arc From Center or
  From 3 Points).
- You are drawing a cam profile, a gear tooth, or any shape whose outline follows a
  circular arc.
- You need an elliptical slot or transition curve (Elliptical Arc).
- You are designing optical elements (mirrors, lenses) that require exact conic
  sections (Hyperbolic Arc, Parabolic Arc).

## When NOT to use it

- **You need a complete circle** — use [Create Circle](create-circle.md) instead.
- **You need a smooth S-curve or free-form blend** — use
  [Create B-Spline](create-bspline.md).
- **You want to chamfer two lines** — use [Create Fillet](create-fillet.md) or
  [Create Chamfer](create-fillet.md#chamfer) which automatically constrain
  the transition.

---

## Step-by-step walkthrough

### Arc From Center

1. **Press `G, A`** or select **Arc From Center** from the toolbar dropdown.
2. **Click the centre point.**
3. **Click the arc start point** (sets the radius and start angle).
4. **Click the arc end point** (sets the end angle; the arc sweeps CCW by default).
5. **Constrain the arc** — add radius, centre position, and angular constraints.

### Arc From 3 Points

1. **Press `G, 3, A`** or select **Arc From 3 Points**.
2. **Click the first endpoint** of the arc.
3. **Click the second endpoint** (the arc will span between these two points).
4. **Click a third point** that lies on the arc body between the endpoints. FreeCAD
   computes the unique circle through these three points and draws the arc.
5. **Constrain** as needed.

### Elliptical Arc

1. **Press `G, E, A`**.
2. **Click the ellipse centre.**
3. **Click a point on the major semi-axis** (sets the major radius direction and length).
4. **Click a point to set the minor radius** (perpendicular drag).
5. **Click the arc start point and end point** to define the angular sweep.

!!! tip
    Use a [Symmetry](constraint-symmetric.md) or [Equal](constraint-equal.md)
    constraint on the two semi-axes to make the ellipse a circle; or control both
    radii with [Radius](constraint-radius-diameter.md) constraints.

---

## Parameters

### Arc From Center / Arc From 3 Points

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) |
| Centre position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) |
| Start / end angle | Angle | [Angle](constraint-angle.md) on endpoint reference |

### Elliptical Arc

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Major radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) |
| Minor radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) |
| Major axis angle | Angle | [Angle](constraint-angle.md) |
| Centre position | Coordinate | [Lock Position](constraint-lock.md) |

---

## Elliptical arc

An elliptical arc is defined by its centre, major axis direction, major and minor
radii, and the start/end angles of the arc portion. It is stored as a
`Part::GeomArcOfEllipse`.

Internally, an elliptical arc has two *focus* points (special geometry attached to
the arc). These are visible as small construction points; they participate in
constraints and can be used to constrain the eccentricity of the ellipse.

## Hyperbolic and parabolic arcs

These are mathematically precise conic sections, not approximations. They are
most useful in optical design (parabolic mirrors, hyperbolic lenses) and in
architectural geometry (hyperbolic cooling towers, parabolic arches).

**Hyperbolic arc** is defined by centre, one vertex of the hyperbola, and a point
on the arc. Two focus points are also available as constraint handles.

**Parabolic arc** is defined by the focal point, a point on the directrix, and a
point on the parabola. The focus is the primary constraint handle.

---

## Advanced usage

### Mixing arcs and lines in a Polyline

Inside [Polyline](create-line.md) mode, press `M` to toggle between line and arc
sub-modes. The junction between a line and an arc in Polyline mode automatically
gets a Tangent constraint, giving you smooth blended corners.

### Toggling arc direction

During Arc From Center placement, clicking on the opposite side of the centre
from the intended sweep reverses the arc direction (CW vs CCW). After creation,
edit the arc endpoints to change which sector is kept.

---

## Common mistakes and pitfalls

!!! warning "Arc is the wrong portion of the circle"
    **Cause:** The arc sweeps the larger sector when you expected the smaller one
    (or vice versa).  
    **Fix:** Select the arc, open its properties, and swap the `StartAngle` and
    `EndAngle`; or delete and re-draw clicking in a different order.

!!! warning "Elliptical arc focus points show as red (conflicting)"
    **Cause:** The focus points have been over-constrained by coincident or distance
    constraints that conflict with the ellipse geometry.  
    **Fix:** Remove duplicate constraints on the focus points — they are derived from
    the radii and should generally be left unconstrained unless you specifically want
    to drive the eccentricity via the foci.

---

## Python API

### Minimal example — circular arc

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Arc from center (0,0), radius 10, from 0° to 90°
import math
arc = Part.ArcOfCircle(
    Part.Circle(App.Vector(0, 0, 0), App.Vector(0, 0, 1), 10),
    0,           # start angle (radians)
    math.pi / 2  # end angle
)
idx = sketch.addGeometry(arc, False)
# Constrain centre to origin
sketch.addConstraint(Sketcher.Constraint("Coincident", idx, 3, -1, 1))
# Fix radius
sketch.addConstraint(Sketcher.Constraint("Radius", idx, 10.0))
doc.recompute()
```

### Key geometry properties

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `App::Vector` | Centre of the parent circle |
| `Radius` | float | Radius in mm |
| `StartAngle` | float | Arc start in radians |
| `EndAngle` | float | Arc end in radians |

---

## See also

- [Create Circle](create-circle.md) — full circles and ellipses
- [Create Line](create-line.md) — straight segments to combine with arcs
- [Create B-Spline](create-bspline.md) — free-form curves
- [Create Fillet](create-fillet.md) — add a circular arc transition between two lines
- [Radius/Diameter](constraint-radius-diameter.md) — constrain arc radius
