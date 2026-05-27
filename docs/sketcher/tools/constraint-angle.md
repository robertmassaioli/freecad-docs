# Angle Constraint

> **In one sentence:** Fix the angle between two lines (or between a line and the
> sketch X-axis) to a specific value, in driving or reference mode.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Angle Dimension  
**Toolbar:** Sketcher Constraints · **Shortcut:** `K, A`

Angle removes 1 degree of freedom: the relative direction between two lines,
or the absolute orientation of one line with respect to the sketch X-axis.

---

## Intuition

Think of a protractor measuring the angle between two edges meeting at a point.
Angle Constraint is the protractor that also *locks* the measurement: the angle
can no longer change freely once the constraint is applied.

---

## When to use it

- Forcing two lines to meet at a specific angle (e.g. 60° for an equilateral
  triangle, 45° for a chamfer).
- Fixing the absolute orientation of a single line relative to the sketch X-axis
  (e.g. a slanted profile edge at 30°).
- Setting the included angle of an arc.
- Constraining a regular polygon's orientation by fixing one side's angle.

## When NOT to use it

- **You want exactly 90°** — use [Perpendicular](constraint-perpendicular.md);
  it is cleaner and does not require entering a number.
- **You want exactly 0° or 180° (lines parallel or collinear)** — use
  [Parallel](constraint-parallel.md) or [Tangent](constraint-tangent.md).
- **You want a line horizontal or vertical** — use
  [Horizontal/Vertical](constraint-horizontal-vertical.md).

---

## Step-by-step walkthrough

### Angle between two lines

1. **Select two lines** (++ctrl++-click each).
2. **Press `K, A`** or activate **Angle Dimension**.
3. **Click to place** the angle arc label at the intersection.
4. **Enter the angle value** in degrees and click OK.

!!! tip
    The angle is measured from the *first selected line* to the *second selected
    line*, in the counterclockwise direction. If the result is the supplement of
    what you expected, select in the opposite order or add 180°.

### Absolute angle of a single line

1. **Select one line.**
2. **Press `K, A`.**
3. The angle is measured from the sketch X-axis to the line direction (CCW positive).
4. Enter the value (e.g. 30 for 30° above horizontal).

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Angle | Degrees | Current angle | The angle to enforce. Range 0°–360°. |
| Mode | Driving / Reference | Driving | See [Dimension → Driving vs Reference](dimension.md#driving-vs-reference-mode-in-depth). |

---

## Common mistakes and pitfalls

!!! warning "Angle constraint gives the supplement angle instead of intended"
    **Cause:** The angle is measured between the line *directions* (vectors), not
    between the lines themselves. Selecting lines in a different order or with
    different directions can flip whether you get θ or 180°−θ.  
    **Fix:** If you get 150° when you expected 30°, change the value to 30° — the
    solver will flip the geometry accordingly — or reselect in the other order.

!!! warning "Angle in reference mode reads differently than expected"
    **Cause:** The reference angle is computed from the line vectors as drawn;
    if the lines have been trimmed, the vector direction may be opposite to what
    you expect.  
    **Fix:** Check the arc direction marker (the small arc shown with the label)
    to confirm which angle is being read.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part
import math

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

line0 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(10, 0, 0)), False)
line1 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(7, 7, 0)), False)

# Angle of 45° between the two lines (value in radians internally)
sketch.addConstraint(Sketcher.Constraint("Angle", line0, 2, line1, 1, math.pi / 4))
doc.recompute()
```

### Method signatures

```python
# Absolute angle of a line from X-axis
sketch.addConstraint(Sketcher.Constraint("Angle", geoId, math.radians(angle_degrees)))

# Angle between two lines at a shared vertex
sketch.addConstraint(Sketcher.Constraint(
    "Angle", geo1, vertex1, geo2, vertex2, math.radians(angle_degrees)))
```

Note: the solver stores angles in **radians** even though the UI shows degrees.

---

## See also

- [Dimension](dimension.md) — unified tool; auto-detects angle when two lines are selected
- [Perpendicular](constraint-perpendicular.md) — 90° shortcut (no number required)
- [Horizontal/Vertical](constraint-horizontal-vertical.md) — 0° / 90° axis-aligned shortcuts
- [Parallel](constraint-parallel.md) — equal angle shortcut for parallel lines
