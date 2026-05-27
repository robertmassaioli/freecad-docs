# Rectangle / Centered Rectangle / Rounded Rectangle

> **In one sentence:** Draw an axis-aligned rectangle (or an oblong with rounded
> ends) in one step, with all four sides automatically added and the corners
> automatically constrained to be right angles.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Rectangle  
**Toolbar:** Sketcher Geometries (Rectangle dropdown) · **Shortcut:** `G, R` (default: Rectangle)

Three rectangle variants share one toolbar dropdown:

| Tool | Shortcut | Description |
|------|----------|-------------|
| Rectangle | `G, R` | Click two opposite corners |
| Centered Rectangle | `G, V` | Click the centre, then one corner |
| Rounded Rectangle | `G, O` | Click two opposite corners; a radius input sets the corner arcs |

All three automatically add Right Angle constraints at the corners so the outline
is always truly rectangular. You then add width, height, and position constraints
to fully define the shape.

---

## Intuition

Drawing a rectangle manually (four lines + four right-angle constraints + four
coincident constraints at corners) is tedious. The Rectangle tools do all of that
in two clicks, giving you a closed, properly constrained rectangle that only needs
its width and height set.

Centered Rectangle is useful when you know where the *middle* of the rectangle
should be (e.g. centred on a hole pattern) rather than a corner. Rounded Rectangle
adds arc fillets at all four corners simultaneously, useful for rounded slots,
panel cutouts, or organic outlines.

---

## When to use it

- You need any rectangular profile — plates, pads, pockets, slots.
- The rectangle must be aligned with the sketch's X/Y axes (all tools here produce
  axis-aligned rectangles; for a rotated rectangle, draw lines and add angle
  constraints manually).
- You want rounded corners from the start rather than adding fillets afterwards
  (Rounded Rectangle).

## When NOT to use it

- **You need a rotated rectangle** — draw four lines and apply angle + parallel
  constraints manually.
- **You need a rectangle that is part of a larger polyline** — use
  [Polyline](create-line.md) to draw the sides one by one so you can continue
  the chain.
- **You need only the shape of a slot (two semicircles and two straight sides)** —
  use [Create Slot](create-slot.md) which creates a proper slotted profile.

---

## Step-by-step walkthrough

### Rectangle (corner-to-corner)

1. **Press `G, R`** or select **Rectangle** from the dropdown.
2. **Click one corner** of the rectangle.
3. **Click the opposite corner.** Four lines appear, corners are right-angle
   constrained, and all four sides are connected with coincident constraints.
4. **Add width and height constraints** using [Dimension](dimension.md) or
   [Horizontal/Vertical Distance](constraint-distance.md).
5. **Fix the position** — constrain one corner or the centre to the origin or a
   known point.

### Centered Rectangle

1. **Press `G, V`** or select **Centered Rectangle**.
2. **Click the desired centre point.**
3. **Click one corner.** The rectangle is drawn symmetrically around the centre.
4. **Constrain width, height, and centre position.**

!!! tip
    Centered Rectangle automatically centres the shape, but it does *not* add a
    symmetry constraint about the sketch origin. Add a [Lock Position](constraint-lock.md)
    or [Coincident](constraint-coincident.md) to the centre point to fix it.

### Rounded Rectangle (Oblong)

1. **Press `G, O`** or select **Rounded Rectangle**.
2. **Click two opposite corners** of the bounding box.
3. A dialog or input field requests the **corner radius**. Enter the radius value
   and confirm.
4. The rounded rectangle appears: four line sides with arc fillets at each corner.
5. **Constrain** width, height, corner radius, and position.

---

## Parameters

### Rectangle / Centered Rectangle

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Width | Distance | [Horizontal Distance](constraint-distance.md#horizontal-distance) |
| Height | Distance | [Vertical Distance](constraint-distance.md#vertical-distance) |
| Position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) on a corner or centre |

### Rounded Rectangle

All parameters above, plus:

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Corner radius | Distance | [Radius/Diameter](constraint-radius-diameter.md) applied to any corner arc |

---

## Advanced usage

### Constraining all four sides at once

After drawing a rectangle, you can select opposite sides and apply an
[Equal](constraint-equal.md) constraint to enforce that width equals height (making
a square). Or select all four arcs in a Rounded Rectangle and apply Equal to ensure
uniform corner radii.

### Using the centre point of a Rectangle

A rectangle drawn with the plain Rectangle tool does *not* have an explicit centre
point. To constrain the centre, add a [Point](create-point.md) at the intersection
of its diagonals and use [Coincident](constraint-coincident.md) to connect it to
the desired reference. Alternatively, use Centered Rectangle from the start.

---

## Common mistakes and pitfalls

!!! warning "Rectangle is slightly off-axis (not perfectly horizontal/vertical)"
    **Cause:** The two corner clicks were not exactly aligned, producing a
    near-horizontal (but not quite) rectangle.  
    **Fix:** The right-angle constraints at corners ensure the angles are exactly 90°,
    but the rectangle can still be rotated as a whole. Add a
    [Horizontal](constraint-horizontal-vertical.md) constraint to one side to lock
    it to the X-axis.

!!! warning "Cannot add a width constraint — already over-constrained"
    **Cause:** Both corner X-coordinates were already fixed by Lock Position
    constraints.  
    **Fix:** Remove one of the X-position constraints and replace it with the width
    constraint.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Draw a 30 × 20 rectangle with bottom-left corner at origin
w, h = 30.0, 20.0
# Four corners
pts = [
    App.Vector(0, 0, 0), App.Vector(w, 0, 0),
    App.Vector(w, h, 0), App.Vector(0, h, 0)
]
# Four edges
ids = []
for i in range(4):
    ids.append(sketch.addGeometry(
        Part.LineSegment(pts[i], pts[(i+1) % 4]), False))

# Join corners
for i in range(4):
    sketch.addConstraint(Sketcher.Constraint(
        "Coincident", ids[i], 2, ids[(i+1) % 4], 1))

# Horizontal and vertical constraints
sketch.addConstraint(Sketcher.Constraint("Horizontal", ids[0]))
sketch.addConstraint(Sketcher.Constraint("Horizontal", ids[2]))
sketch.addConstraint(Sketcher.Constraint("Vertical",   ids[1]))
sketch.addConstraint(Sketcher.Constraint("Vertical",   ids[3]))

# Width and height
sketch.addConstraint(Sketcher.Constraint("Distance", ids[0], w))
sketch.addConstraint(Sketcher.Constraint("Distance", ids[1], h))

# Pin bottom-left corner to origin
sketch.addConstraint(Sketcher.Constraint("Coincident", ids[0], 1, -1, 1))

doc.recompute()
```

---

## See also

- [Create Line](create-line.md) — for non-axis-aligned rectangles
- [Create Regular Polygon](create-regular-polygon.md) — squares and other polygons
- [Create Slot](create-slot.md) — slotted profiles
- [Create Fillet](create-fillet.md) — add rounded corners to an existing rectangle
- [Horizontal/Vertical Distance](constraint-distance.md) — set width and height
