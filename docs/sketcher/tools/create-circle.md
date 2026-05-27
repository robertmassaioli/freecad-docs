# Circles and Ellipses

> **In one sentence:** Draw complete circles or ellipses — by specifying their
> centre and a point on their perimeter, or by specifying three points that the
> curve passes through.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Circle / Ellipse  
**Toolbar:** Sketcher Geometries (Conic dropdown) · **Shortcut:** `G, C` (default: Circle from Center)

The Conic group provides four tools for drawing closed conic sections:

| Tool | Shortcut | Description |
|------|----------|-------------|
| Circle From Center | `G, C` | Two clicks: centre, then a point on the rim |
| Circle From 3 Points | `G, 3, C` | Three clicks on the circumference |
| Ellipse From Center | `G, E, E` | Three clicks: centre, major-axis end, minor-axis end |
| Ellipse From 3 Points | `G, 3, E` | Three clicks: two on the ellipse, one to set minor radius |

---

## Intuition

A circle is the simplest closed curve — all points equidistant from the centre.
Circle From Center is the most natural approach: place the centre where you want it,
then drag out to set the radius. Circle From 3 Points is useful when you know three
points the circle must pass through (e.g. three hole centres that define a bolt
circle) and don't want to compute the centre yourself.

An ellipse is a stretched circle — it has a long axis (major) and a short axis
(minor). Ellipse From Center asks you to place the centre, then click to establish
the major axis direction and length, then click again for the minor axis.

---

## When to use it

- You need a circular boss, hole profile, slot end-cap, or any round feature.
- You need an elliptical extrusion or decorative oval shape.
- Three existing geometry points define the circle you need (Circle From 3 Points).

## When NOT to use it

- **You only need part of a circle** — use [Create Arc](create-arc.md) for arc
  segments.
- **You need a circle that is automatically constrained to pass through a specific
  point** — draw the circle then add a [Point on Object](constraint-point-on-object.md)
  or [Coincident](constraint-coincident.md) constraint.
- **You need an oblong or rounded rectangle** — use
  [Create Rectangle → Rounded Rectangle](create-rectangle.md#rounded-rectangle).

---

## Step-by-step walkthrough

### Circle from Center

1. **Press `G, C`** or select **Circle From Center** from the toolbar dropdown.
2. **Click the centre point.**
3. **Click a point on the circumference** (or type a radius in the toolbar
   input field). The full circle appears.
4. **Constrain the circle** — add a [Radius/Diameter](constraint-radius-diameter.md)
   constraint to fix its size, and a [Coincident](constraint-coincident.md) or
   [Lock Position](constraint-lock.md) constraint to fix its centre.

### Circle from 3 Points

1. **Press `G, 3, C`** or select **Circle From 3 Points**.
2. **Click three points** on the desired circumference. FreeCAD computes the unique
   circle through these three points.
3. **Constrain** if needed — the circle's radius and centre are already defined by
   the three points, so additional radius constraints are redundant unless those
   points are themselves unconstrained.

### Ellipse from Center

1. **Press `G, E, E`** or select **Ellipse From Center**.
2. **Click the ellipse centre.**
3. **Click the end of the major axis** (sets major radius direction and length).
4. **Click a point** to set the minor radius (drag perpendicular to the major axis).
5. **Constrain** the major and minor radii with
   [Radius/Diameter](constraint-radius-diameter.md) constraints.

---

## Parameters

### Circle

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) |
| Centre position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) |

### Ellipse

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Major radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) applied to the major-axis edge |
| Minor radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) applied to the minor-axis edge |
| Major axis angle | Angle | [Angle](constraint-angle.md) or [Horizontal](constraint-horizontal-vertical.md) |
| Centre position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) |

---

## Ellipse

An ellipse in Sketcher is stored as a `Part::GeomEllipse` with two focus points
that are exposed as special construction vertices. The focus points can be
constrained to specific positions if you want to drive the ellipse eccentricity
by specifying the focal distance rather than the minor radius.

For many practical use cases (oval boss, decorative shape) simply constrain the
major and minor radii directly and position the centre.

---

## Common mistakes and pitfalls

!!! warning "Diameter vs radius confusion"
    **Cause:** When you apply a [Radius/Diameter](constraint-radius-diameter.md)
    constraint to a circle, the tool auto-selects whether to show radius or diameter
    based on the circle's current appearance. Entering `10` when diameter mode is
    active gives a circle of diameter 10 (radius 5), not radius 10.  
    **Fix:** Check the constraint label — `R 10` means radius 10, `Ø 10` means
    diameter 10. Use the **R** (radius) shortcut `K, R` or **Ø** (diameter) shortcut
    `K, O` to force the mode you want.

!!! warning "Ellipse focus-point constraints conflict with radius constraints"
    **Cause:** Adding coincident constraints to the focus points while also
    constraining major and minor radii can over-constrain the ellipse.  
    **Fix:** Constrain either the foci or the radii — not both. For most designs,
    constrain the radii and leave the foci unconstrained.

---

## Python API

### Minimal example — circle

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Draw a circle radius 15 centred at (20, 10)
circle = Part.Circle(App.Vector(20, 10, 0), App.Vector(0, 0, 1), 15)
idx = sketch.addGeometry(circle, False)

# Fix the radius
sketch.addConstraint(Sketcher.Constraint("Radius", idx, 15.0))
# Fix the centre with X/Y distance constraints from origin
sketch.addConstraint(Sketcher.Constraint("DistanceX", idx, 3, -1, 1, 20.0))
sketch.addConstraint(Sketcher.Constraint("DistanceY", idx, 3, -1, 1, 10.0))
doc.recompute()
```

### Key geometry properties

`Part.Circle` returned from `sketch.Geometry[idx]`:

| Property | Type | Description |
|----------|------|-------------|
| `Center` | `App::Vector` | Circle centre coordinates |
| `Radius` | float | Radius in mm |

`Part.Ellipse` has additional `MajorRadius`, `MinorRadius`, `AngleXU` (major axis angle).

---

## See also

- [Create Arc](create-arc.md) — arc (portion of a circle or conic)
- [Radius/Diameter](constraint-radius-diameter.md) — constrain circle or arc size
- [Create Rectangle](create-rectangle.md) — rectangular profiles
- [Constraint Equal](constraint-equal.md) — force two circles to have the same radius
