# Perpendicular Constraint

> **In one sentence:** Force two lines (or a line and a curve) to meet at exactly
> 90°, removing one rotational degree of freedom from the pair.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Perpendicular Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `N`

Perpendicular removes 1 DoF — the relative angle between the two elements — and
fixes it at 90°. It does not require the two elements to actually intersect; it
only constrains their *directions* to be orthogonal.

---

## Intuition

Think of a draughtsman's set square: one arm always points exactly 90° from the
other, regardless of which way you rotate the square. Perpendicular is that
90° relationship, permanently encoded as a constraint.

---

## When to use it

- You need a right-angle corner that is not produced by the Rectangle tool (e.g. an
  L-shaped profile drawn as a polyline).
- A line should meet a circle's radius (line through centre perpendicular to tangent).
- Two datum axes in a construction layout must be orthogonal.
- A construction line should be perpendicular to an edge for use as a normal or
  offset reference.

## When NOT to use it

- **You need a right-angle *and* an intersection** — Perpendicular alone does not
  make lines meet; also add [Coincident](constraint-coincident.md) at the desired
  meeting point.
- **You used Rectangle** — Rectangle already applies right-angle constraints at
  all four corners; do not add extra Perpendicular constraints.

---

## Step-by-step walkthrough

1. **Select two lines** (or a line and an arc/circle).
2. **Press `N`** or activate **Sketch → Perpendicular Constraint**.
3. The selected elements rotate to be perpendicular. The constraint icon appears
   at the intersection or between the elements.

!!! tip
    When applied to a line and a circle, Perpendicular forces the line to be
    tangent (perpendicular to the radius at the tangent point). Combined with
    [Tangent](constraint-tangent.md), this gives you precise tangent-line
    constructions.

---

## Parameters

No numeric parameters. Removes 1 DoF.

---

## Common mistakes and pitfalls

!!! warning "Lines do not intersect despite being perpendicular"
    **Cause:** Perpendicular only constrains the *angle*, not the position. Two
    lines at 90° can still be anywhere relative to each other.  
    **Fix:** Add a [Coincident](constraint-coincident.md) constraint or [Point on Object](constraint-point-on-object.md)
    to make them actually meet.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

line0 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(20, 0, 0)), False)
line1 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(0, 20, 0)), False)

sketch.addConstraint(Sketcher.Constraint("Perpendicular", line0, line1))
doc.recompute()
```

---

## See also

- [Parallel](constraint-parallel.md) — force two lines to be parallel
- [Tangent](constraint-tangent.md) — smooth curve-to-curve or line-to-curve joins
- [Horizontal/Vertical](constraint-horizontal-vertical.md) — axis-aligned constraints
- [Angle](constraint-angle.md) — set an arbitrary angle between two lines
