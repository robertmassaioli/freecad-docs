# Horizontal / Vertical / HorVer Constraint

> **In one sentence:** Force a line to be exactly horizontal, exactly vertical, or
> automatically whichever of the two it is closest to — removing the line's
> rotational degree of freedom.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Horizontal/Vertical Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `H` (Horizontal) · `V` (Vertical) · `A` (HorVer auto)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Horizontal | `H` | Force selected line(s) to be parallel to the sketch X-axis |
| Vertical | `V` | Force selected line(s) to be parallel to the sketch Y-axis |
| HorVer (auto) | `A` | Apply Horizontal or Vertical based on which is closer to the current orientation |

All three remove 1 degree of freedom (the rotational orientation of the line).
For a pair of points instead of a line, the constraint forces the two points to
share the same Y coordinate (Horizontal) or same X coordinate (Vertical).

---

## Intuition

A newly drawn line can rotate freely around its midpoint. Horizontal snaps it
flat — like a spirit level indicating the line is perfectly level. Vertical snaps
it upright, like a plumb line. HorVer is a shortcut that reads the current
angle and applies the nearest option, saving you a decision.

---

## When to use it

- Any line that should be a true horizontal edge (shelf, table top, floor line).
- Any line that should be a true vertical edge (wall, column, post).
- After drawing a line at approximately the right angle, use `A` to snap it to
  the exact axis without having to decide H or V yourself.
- To force the flat side of a hexagon to be horizontal (apply Horizontal to one
  of the flat sides).

## When NOT to use it

- **You want the line at a specific non-axis angle** — use
  [Angle](constraint-angle.md) instead.
- **You want two lines to be parallel to each other (not necessarily to an axis)**
  — use [Parallel](constraint-parallel.md).

---

## Step-by-step walkthrough

1. **Select one or more lines** (or pairs of points for point-to-point mode).
2. **Press `H`, `V`, or `A`** depending on the desired axis.
3. The line(s) snap to the chosen orientation immediately. The constraint icon
   appears next to each affected line.

!!! tip
    You can apply Horizontal or Vertical *while drawing* a Polyline by pressing
    `H` or `V` at a corner — this locks the segment currently being drawn to the
    axis before you click the next point.

---

## Parameters

No numeric parameters. The constraint removes exactly 1 DoF per line.

| Constraint | DoF removed | Requirement |
|-----------|-------------|-------------|
| Horizontal (on a line) | 1 | Line becomes parallel to X-axis |
| Vertical (on a line) | 1 | Line becomes parallel to Y-axis |
| Horizontal (on two points) | 1 | Points share the same Y coordinate |
| Vertical (on two points) | 1 | Points share the same X coordinate |

---

## Common mistakes and pitfalls

!!! warning "Constraint makes the sketch over-constrained"
    **Cause:** The line was already forced to be horizontal/vertical by a combination
    of existing constraints (e.g. both endpoints had Lock Position constraints that
    implied the same Y coordinate).  
    **Fix:** Remove one of the redundant Lock Position constraints and rely on the
    Horizontal/Vertical constraint to enforce the axis alignment.

!!! warning "HorVer applied Horizontal when you wanted Vertical"
    **Cause:** The line was drawn closer to horizontal than to vertical.  
    **Fix:** Press ++ctrl+z++ to undo, rotate the line closer to vertical (drag an
    endpoint), then press `A` again — or use `V` directly.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Add a line and constrain it horizontal
idx = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 5, 0), App.Vector(20, 5.3, 0)), False)

sketch.addConstraint(Sketcher.Constraint("Horizontal", idx))
doc.recompute()
# The line snaps to Y=5 (the start Y), removing the slight tilt.
```

### Method signatures

```python
# Horizontal on a line
sketch.addConstraint(Sketcher.Constraint("Horizontal", geoId))

# Vertical on a line
sketch.addConstraint(Sketcher.Constraint("Vertical", geoId))

# Horizontal between two points (same Y)
sketch.addConstraint(Sketcher.Constraint("Horizontal", geo1, vertex1, geo2, vertex2))

# Vertical between two points (same X)
sketch.addConstraint(Sketcher.Constraint("Vertical", geo1, vertex1, geo2, vertex2))
```

---

## See also

- [Parallel](constraint-parallel.md) — force two lines to be parallel to each other
- [Angle](constraint-angle.md) — set an arbitrary angle
- [Perpendicular](constraint-perpendicular.md) — force two lines to meet at 90°
- [Dimension](dimension.md) — the unified tool that can infer horizontal/vertical from context
