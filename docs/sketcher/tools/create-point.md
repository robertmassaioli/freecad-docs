# Point

> **In one sentence:** Place a standalone reference point in the sketch that can
> serve as a constraint anchor, a construction aid, or a vertex for other tools.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → Point  
**Toolbar:** Sketcher Geometries · **Shortcut:** `G, Y`

Point creates a single 2-D point in the active sketch. The point has no length or
area — it is purely a positional marker. Despite its simplicity, it is useful as
a constraint attachment target and as a centre reference for arcs or polygons that
you want to position relative to something other than the sketch origin.

---

## Intuition

Think of a pin pushed into a drawing board: the pin has no size but it anchors
a position. Other geometry can be constrained to coincide with that pin, and the
pin itself can be constrained to a specific coordinate or to lie on an edge.

---

## When to use it

- You need a named reference point to which other geometry will be constrained
  (e.g. the centre of a pattern, the pivot of a rotation).
- You want to place a point that a B-Spline or Polyline will pass through.
- You are building a construction layout and need intersections that are not
  naturally produced by existing geometry.

## When NOT to use it

- **You want to draw a line that starts from a specific location** — draw the line
  and add a [Coincident](constraint-coincident.md) or
  [Lock Position](constraint-lock.md) constraint to its endpoint instead.
- **You want the origin** — the sketch origin (at coordinate 0, 0) already exists
  as a built-in special point; select it with **Sketch → Select Origin**.

---

## Step-by-step walkthrough

1. **Enter sketch edit mode.**
2. **Press `G, Y`** or activate **Sketch → Sketcher geometries → Point**.
3. **Click in the viewport** to place the point. The point appears as a small
   cross.
4. **Press `Esc` or right-click** to leave point-placement mode.
5. **Constrain the point** — add [Lock Position](constraint-lock.md) or
   [Coincident](constraint-coincident.md) constraints to remove its remaining
   2 degrees of freedom.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Position (click) | 2-D coordinate | cursor | Where the point is placed; exact position is set by subsequent constraints. |
| Construction | Toggle | Off | If on, the point is a construction (reference) element and does not contribute to the sketch profile. |

---

## Common mistakes and pitfalls

!!! warning "Point adds 2 DoF to the sketch"
    **Cause:** Every new point adds two unconstrained degrees of freedom (X and Y).
    If you place points for reference but do not constrain them, the sketch will be
    under-constrained.  
    **Fix:** Always constrain placed points with at least a Lock Position, two
    distance constraints, or a Coincident + one more constraint.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Add a point at (10, 20)
idx = sketch.addGeometry(Part.Point(App.Vector(10, 20, 0)), False)
print(f"Point added at geometry index {idx}")

# Constrain it with absolute X and Y (Lock Position equivalent)
sketch.addConstraint(Sketcher.Constraint("DistanceX", idx, 1, 10.0))
sketch.addConstraint(Sketcher.Constraint("DistanceY", idx, 1, 20.0))
doc.recompute()
```

### Resulting feature properties

A Point geometry element has no special sub-properties beyond its coordinates,
which are accessed via `sketch.Geometry[idx].StartPoint`.

---

## See also

- [Coincident](constraint-coincident.md) — attach two points together
- [Lock Position](constraint-lock.md) — fix a point to absolute X, Y coordinates
- [Create Line](create-line.md) — a line is defined by two points
