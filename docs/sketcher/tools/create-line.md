# Line / Polyline

> **In one sentence:** Draw a single straight line segment (Line) or a connected
> chain of line segments where each endpoint automatically becomes the start of the
> next (Polyline).

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Line / Polyline  
**Toolbar:** Sketcher Geometries · **Shortcut:** `G, L` (Line) · `G, M` (Polyline)

Line and Polyline are the most fundamental drawing tools in Sketcher. A **Line**
creates one segment; a **Polyline** creates a chain of segments, automatically
adding a Coincident constraint between consecutive endpoints. Both tools leave the
resulting geometry unconstrained until you apply length, angle, and positional
constraints.

---

## Intuition

A Line is like drawing a single stroke of a pencil between two points. A Polyline
is like keeping your pencil on the paper and making a series of strokes — each new
stroke starts exactly where the previous one ended, so the chain is always
connected.

The key practical difference: if you draw a closed shape (a rectangle, a triangle)
using Polyline, the last click near the first vertex will close the loop
automatically. Drawing the same shape with individual Line commands requires you
to manually add Coincident constraints at each junction.

---

## When to use it

- You need any straight-sided shape: profiles, cross-sections, brackets, plates.
- You want to draw a connected outline quickly without manually applying coincident
  constraints at every corner (Polyline).
- You want a single construction axis or reference line (Line in construction mode).

## When NOT to use it

- **You need a rectangle** — use [Create Rectangle](create-rectangle.md) instead;
  it draws a closed, constrained rectangle in one step.
- **You need a regular polygon** — use [Create Regular Polygon](create-regular-polygon.md).
- **You want to connect two existing endpoints** — you can still use Line, but adding
  a Coincident constraint to each endpoint after drawing is equivalent and more
  explicit.

---

## Step-by-step walkthrough

### Single line

1. **Press `G, L`** or activate **Sketch → Sketcher geometries → Line**.
2. **Click the start point** in the viewport.
3. **Click the end point.** A line segment appears; a length dimension is not yet
   applied.
4. **Press `Esc` or right-click** to exit line mode, or click a new start point to
   draw another separate line.
5. **Constrain the line** — set its length with [Dimension](dimension.md) or
   [Distance](constraint-distance.md), and its angle or endpoint positions.

### Polyline (chain of connected lines)

1. **Press `G, M`** or activate **Sketch → Sketcher geometries → Polyline**.
2. **Click the start point.**
3. **Click successive waypoints.** Each click completes the previous segment and
   starts the next; a Coincident constraint is applied automatically at each
   junction.
4. **To close the polyline:** click near (or exactly on) the starting point. A
   closing line with a Coincident constraint appears.
5. **Press `Esc` or right-click** to finish without closing, leaving the last point
   unconnected.

!!! tip
    While drawing a Polyline, press `M` to toggle between **Line** and **Arc**
    mode. This lets you mix straight segments and arc segments in a single connected
    chain without leaving the command.

---

## Parameters

Both tools share the same implicit parameter: the two endpoints set by clicks.
After drawing:

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Length | Distance | [Dimension](dimension.md) or [Distance](constraint-distance.md) |
| Angle | Angle | [Dimension](dimension.md) or [Angle](constraint-angle.md) |
| Endpoint position | Coordinate | [Lock Position](constraint-lock.md) or [Coincident](constraint-coincident.md) |
| Horizontal / Vertical | Geometric | [Horizontal/Vertical](constraint-horizontal-vertical.md) |

---

## Advanced usage

### Mixed line-arc polyline

Inside Polyline mode, pressing `M` toggles between straight-line and arc
sub-modes. In arc mode, three clicks define an arc instead of a line. The tangency
at the junction is automatically maintained as a Tangent constraint, giving you
smooth blended outlines in a single operation.

### Construction lines

While drawing (or after selecting a line), press `G, W` or toggle the
**Construction** mode button in the toolbar to convert the line to a construction
(dashed) element. Construction lines participate in constraints but do not form
part of the sketch profile passed to Part Design.

---

## Common mistakes and pitfalls

!!! warning "Polyline not closed — last segment floats"
    **Cause:** You pressed Esc before clicking back on the first vertex, leaving the
    last endpoint free.  
    **Fix:** Add a [Coincident](constraint-coincident.md) constraint between the
    last endpoint and the first vertex, or delete the unwanted last segment and
    redraw ending exactly on the first point.

!!! warning "Line length cannot be set — over-constrained"
    **Cause:** Both endpoints are already fully constrained (e.g. each is coincident
    with another fully constrained vertex), so adding a length constraint would be
    redundant.  
    **Fix:** Remove one of the endpoint constraints and replace it with the length
    constraint; or accept that the length is derived from the endpoint positions.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Add a horizontal line from (0,0) to (30,0)
idx = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(30, 0, 0)), False)

# Constrain start to origin
sketch.addConstraint(Sketcher.Constraint("Coincident", idx, 1, -1, 1))
# Fix the length
sketch.addConstraint(Sketcher.Constraint("Distance", idx, 30.0))
# Enforce horizontal
sketch.addConstraint(Sketcher.Constraint("Horizontal", idx))
doc.recompute()
```

### Key properties

`Part.LineSegment` returned by `sketch.Geometry[idx]`:

| Property | Type | Description |
|----------|------|-------------|
| `StartPoint` | `App::Vector` | Coordinates of the first endpoint (vertex 1). |
| `EndPoint` | `App::Vector` | Coordinates of the second endpoint (vertex 2). |
| `Length` | float | Current length in mm; read-only (set via constraints). |

---

## See also

- [Create Rectangle](create-rectangle.md) — draw a closed, constrained rectangle faster
- [Create Arc](create-arc.md) — curved segments for mixed outlines
- [Dimension](dimension.md) — set line length and angle
- [Horizontal/Vertical](constraint-horizontal-vertical.md) — force a line to be axis-aligned
