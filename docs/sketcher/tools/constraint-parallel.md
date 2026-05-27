# Parallel Constraint

> **In one sentence:** Force two lines to be parallel — to run in the same
> direction at any angle, without specifying what that angle is.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Parallel Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `P`

Parallel removes 1 degree of freedom: the relative angle between the two lines.
After applying it, the two lines will always have the same orientation; you can
then set the shared angle with a single [Angle](constraint-angle.md) constraint
applied to either line.

---

## Intuition

Think of railway tracks: both rails run in the same direction, always the same
distance apart. Parallel is the constraint version of "these two things go the
same way" — wherever you point one, the other follows the same bearing.

---

## When to use it

- You have two edges that should remain parallel while other dimensions change
  (e.g. the two sides of a channel).
- You want to force a line to be parallel to a reference edge such as an external
  geometry projection.
- You are building a parallelogram or trapezoid outline.

## When NOT to use it

- **Both lines should be horizontal or vertical** — use
  [Horizontal/Vertical](constraint-horizontal-vertical.md) on each line separately;
  two horizontal lines are automatically parallel.
- **The lines must be parallel AND a specific distance apart** — add a
  [Distance](constraint-distance.md) constraint between them after Parallel.

---

## Step-by-step walkthrough

1. **Select two lines** — click the first, then ++ctrl++-click the second.
2. **Press `P`** or activate **Sketch → Parallel Constraint**.
3. The lines rotate to be parallel. One of them will stay fixed; the other
   adjusts to match its angle.
4. **Add an angle constraint** to either line to fix the shared orientation
   if needed.

---

## Parameters

No numeric parameters. Removes 1 DoF.

---

## Common mistakes and pitfalls

!!! warning "Two fully constrained lines cannot be made parallel"
    **Cause:** Both lines already have all their DoF consumed by existing constraints,
    so adding Parallel is redundant.  
    **Fix:** If the parallelism is implied by the existing constraints, no action is
    needed. If the constraint is genuinely needed, remove one angle-related constraint
    first.

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
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(20, 5, 0)), False)
line1 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 10, 0), App.Vector(20, 15, 0)), False)

sketch.addConstraint(Sketcher.Constraint("Parallel", line0, line1))
doc.recompute()
```

---

## See also

- [Perpendicular](constraint-perpendicular.md) — force two lines to be at 90°
- [Horizontal/Vertical](constraint-horizontal-vertical.md) — force lines to be axis-aligned
- [Angle](constraint-angle.md) — set the angle of a parallel pair
- [Equal](constraint-equal.md) — force two parallel lines to also have equal length
