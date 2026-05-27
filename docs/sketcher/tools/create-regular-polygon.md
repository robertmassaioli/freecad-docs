# Regular Polygon

> **In one sentence:** Draw a regular polygon (equilateral triangle through
> regular octagon, or any N-sided polygon) inscribed in a circle, fully
> constrained with all sides equal and all interior angles equal.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Polygon  
**Toolbar:** Sketcher Geometries (Polygon dropdown) · **Shortcut:** `G, P, 3` (default: Triangle)

The Polygon dropdown contains preset tools for the most common polygons plus a
general tool:

| Tool | Shortcut | Sides |
|------|----------|-------|
| Triangle | `G, P, 3` | 3 |
| Square | `G, P, 4` | 4 |
| Pentagon | `G, P, 5` | 5 |
| Hexagon | `G, P, 6` | 6 |
| Heptagon | `G, P, 7` | 7 |
| Octagon | `G, P, 8` | 8 |
| Regular Polygon (custom) | — | User-specified N |

All variants work the same way: click the centre, click a corner or a midpoint
of a side, and FreeCAD draws the polygon inscribed in the circle through that
second point.

---

## Intuition

Think of drawing a circle, then equally spacing points around it, and connecting
adjacent points with straight lines. Regular Polygon automates this: you set the
centre and the circumscribed circle radius, and FreeCAD draws all the sides and
adds Equal constraints so every side is the same length.

The circle used for construction is drawn as a *construction* element (dashed) —
it exists as a constraint helper but does not form part of the profile that Part
Design extrudes. You can constrain its radius to fix the polygon size.

---

## When to use it

- You need a hex boss, hex socket, or hex flat pattern (Hexagon is the most-used
  variant for M-thread fasteners).
- You need any regular polygon face as a cross-section for a Pad, Pipe, or Loft.
- You want a quick equilateral triangle as a construction reference.

## When NOT to use it

- **You need an irregular polygon** — draw individual lines with angle and distance
  constraints.
- **You need a polygon aligned flat-to-flat rather than point-to-point** — by
  default the polygon is inscribed (circumscribed circle goes through corners).
  For a flat-to-flat width (often needed for hex bolts), apply a
  [Radius/Diameter](constraint-radius-diameter.md) constraint to the
  *construction* midpoint circle and use the inscribed circle radius instead
  (see Advanced usage).

---

## Step-by-step walkthrough

1. **Select the number of sides** from the dropdown (e.g. Hexagon = `G, P, 6`).
   For a custom count, select Regular Polygon and enter the number when prompted.

2. **Click the centre point** of the polygon.

3. **Click a vertex** of the polygon (sets the circumscribed radius and the
   orientation angle). The polygon is drawn immediately.

4. **Constrain the polygon:**
    - Add a [Radius/Diameter](constraint-radius-diameter.md) constraint to the
      construction circle to fix the circumscribed radius (= distance from centre
      to corner).
    - Fix the centre position with [Lock Position](constraint-lock.md) or
      [Coincident](constraint-coincident.md).
    - Fix the orientation by applying a [Horizontal](constraint-horizontal-vertical.md)
      or [Angle](constraint-angle.md) constraint to one side.

!!! tip
    For a hexagon aligned with flat sides horizontal (wrench-flat orientation),
    draw the hexagon and then add a [Horizontal](constraint-horizontal-vertical.md)
    constraint to one of the flat sides.

---

## Parameters

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Number of sides | Integer | Set at creation (dropdown or dialog) |
| Circumscribed radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) on the construction circle |
| Centre position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) |
| Orientation angle | Angle | [Angle](constraint-angle.md) on one side |

---

## Advanced usage

### Flat-to-flat width (inscribed circle)

For hex bolts and similar parts, the key dimension is the *flat-to-flat width*
(the diameter of the inscribed circle, also called the "across-flats" or AF
dimension). The tools here size to the *circumscribed* circle (corner-to-corner).

To constrain the flat-to-flat width:

1. Draw the hexagon as normal.
2. Add a [Dimension](dimension.md) or [Distance](constraint-distance.md) between
   the midpoints of two opposite sides. This sets the flat-to-flat distance.
3. Remove the circumscribed circle radius constraint if you added one (it would be
   redundant).

Alternatively, use the construction circle to represent the *inscribed* circle:
delete the initial constraint on the construction circle, add a Point at a side
midpoint, and constrain that point to be on the construction circle.

### Custom N-sided polygon

Select **Regular Polygon** from the dropdown. A dialog asks for the number of
sides. Enter any integer ≥ 3. The resulting polygon has N sides with Equal
constraints on all sides and internal geometry exactly like the preset tools.

---

## Common mistakes and pitfalls

!!! warning "Polygon rotates unexpectedly when dimensions change"
    **Cause:** The polygon's orientation angle is not constrained — it has one
    remaining rotational degree of freedom.  
    **Fix:** Add a [Horizontal](constraint-horizontal-vertical.md) or
    [Vertical](constraint-horizontal-vertical.md) constraint to one side, or use
    [Angle](constraint-angle.md) to fix the first side's angle.

!!! warning "Circumscribed vs inscribed radius confusion"
    **Cause:** The construction circle passes through the *corners* (circumscribed
    circle). If you type an M10 hex flat-to-flat dimension as the circumscribed
    radius, the polygon will be the wrong size.  
    **Fix:** For flat-to-flat sizing, constrain the midpoint-of-side distance (the
    inscribed circle radius) instead. The two are related by r_inscribed = r_circumscribed × cos(π/N).

---

## Python API

### Minimal example — hexagon

```python
import FreeCAD as App
import Sketcher
import Part
import math

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Hexagon inscribed in radius R, centre at origin, flat-top orientation
R = 10.0
N = 6
ids = []
pts = [App.Vector(R * math.cos(2 * math.pi * i / N + math.pi / 6),
                   R * math.sin(2 * math.pi * i / N + math.pi / 6), 0)
       for i in range(N)]

for i in range(N):
    ids.append(sketch.addGeometry(
        Part.LineSegment(pts[i], pts[(i+1) % N]), False))

# Join corners
for i in range(N):
    sketch.addConstraint(Sketcher.Constraint(
        "Coincident", ids[i], 2, ids[(i+1) % N], 1))

# All sides equal
for i in range(1, N):
    sketch.addConstraint(Sketcher.Constraint("Equal", ids[0], ids[i]))

# Pin centre (first corner distance from origin = R)
# TODO: verify — add circumscribed radius constraint
doc.recompute()
```

---

## See also

- [Create Rectangle](create-rectangle.md) — square / rectangle (4-sided, axis-aligned)
- [Create Circle](create-circle.md) — for the circumscribed or inscribed circle
- [Equal](constraint-equal.md) — force all sides to equal length
- [Radius/Diameter](constraint-radius-diameter.md) — set circumscribed circle size
