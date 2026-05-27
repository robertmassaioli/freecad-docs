# Point on Object Constraint

> **In one sentence:** Force a point to lie somewhere on a line, arc, or circle —
> the point can slide along the edge but cannot leave it.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Point-On-Object Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `O`

Point on Object removes 1 degree of freedom: it forces a point onto an edge's
curve but allows the point to slide along that curve. To fix the point *at a
specific location* on the edge, add a second constraint (e.g. a distance from one
endpoint, or a coincident with a crossing edge).

---

## Intuition

Imagine threading a bead onto a wire: the bead cannot leave the wire, but it is
free to slide back and forth. Point on Object is that wire constraint — the point
is locked onto the edge but not pinned to a specific position on it.

---

## When to use it

- A construction point (centre of pattern, midpoint reference) must lie on a
  specific edge at an unspecified position.
- You want a point to lie on a circle (the circle will pass through the point,
  wherever it is).
- A tangent point between a line and a curve should be at the line's midpoint.
- An external geometry projection should anchor your sketch geometry to a specific
  edge without fixing the exact position.

## When NOT to use it

- **You want two vertices to share the same location** — use
  [Coincident](constraint-coincident.md).
- **You want a point at the midpoint of a line** — apply Point on Object and then
  a [Symmetric](constraint-symmetric.md) constraint about the line's midpoint, or
  add a [Distance](constraint-distance.md) equal to half the line length.

---

## Step-by-step walkthrough

1. **Select a point and an edge** — click the point vertex, then ++ctrl++-click the
   line or arc.
2. **Press `O`** or activate **Sketch → Point-On-Object Constraint**.
3. The point moves to the nearest location on the edge. It can now slide along
   the edge but not leave it.
4. **Add a second constraint** to fix where on the edge the point should lie.

!!! tip
    If the unified [Coincident](constraint-coincident.md) tool (`C`) is used with
    a point-and-edge selection, it automatically applies Point on Object instead.

---

## Parameters

No numeric parameters. Removes 1 DoF.

---

## Common mistakes and pitfalls

!!! warning "Point slides along the edge unexpectedly when other geometry changes"
    **Cause:** Point on Object constrains the point to the edge but leaves 1 DoF
    remaining (position along the edge). If other constraints move the edge, the
    point stays on the edge but may shift to a different location on it.  
    **Fix:** Add a second constraint — a [Distance](constraint-distance.md) from
    one endpoint, or a [Coincident](constraint-coincident.md) with a crossing edge —
    to fix the position along the edge.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

line = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(30, 0, 0)), False)
pt = sketch.addGeometry(Part.Point(App.Vector(15, 0, 0)), False)

# Force point to lie on the line
sketch.addConstraint(Sketcher.Constraint("PointOnObject", pt, 1, line))
doc.recompute()
```

---

## See also

- [Coincident](constraint-coincident.md) — point-to-point join; or point-to-object using the unified tool
- [Lock Position](constraint-lock.md) — fix a point's absolute coordinates
- [Tangent](constraint-tangent.md) — smooth tangency at a point-on-object location
