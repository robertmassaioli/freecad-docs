# Distance Constraints (Distance / Horizontal Distance / Vertical Distance)

> **In one sentence:** Set the straight-line, horizontal, or vertical distance
> between two points or geometry elements, fixing that measurement as a driving
> or reference constraint.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Distance / Horizontal Distance / Vertical Distance  
**Toolbar:** Sketcher Constraints · **Shortcut:** `K, D` (Distance) · `L` (Horizontal) · `I` (Vertical)

| Tool | Shortcut | What it measures |
|------|----------|-----------------|
| Distance | `K, D` | Straight-line distance between two points, or length of a line |
| Horizontal Distance | `L` | Horizontal component (ΔX) between two points, or X-distance from origin |
| Vertical Distance | `I` | Vertical component (ΔY) between two points, or Y-distance from origin |

All three support Driving and Reference modes (see [Dimension](dimension.md) for
the deep dive on this topic).

---

## Intuition

**Distance** is the ruler measurement — the shortest path between two points.

**Horizontal Distance** is how far apart the points are *horizontally*, ignoring
any vertical offset. Think of a spirit level placed between the points and measuring
the horizontal gap.

**Vertical Distance** is the *vertical* gap — like a measuring tape hanging
straight down between the two heights.

These three tools handle the majority of positional constraints you will apply
in everyday sketching. Radius and angle constraints cover the remaining cases.

---

## When to use them

- **Distance:** Set the length of a line; set the diagonal distance between two
  bolt holes; set the clearance between a profile edge and a reference point.
- **Horizontal Distance:** Set the width of a feature; set the X-offset of a hole
  from a reference wall.
- **Vertical Distance:** Set the height of a feature; set the Y-offset from a
  baseline.

## When NOT to use them

- **You want the unified tool** — use [Dimension](dimension.md) (`D`) and press
  `M` to cycle to the desired type.
- **You want to fix absolute coordinates (both X and Y at once)** — use
  [Lock Position](constraint-lock.md) (`K, L`).
- **You want to constrain a radius or diameter** — use
  [Radius/Diameter](constraint-radius-diameter.md).

---

## Step-by-step walkthrough

### Distance (straight-line)

1. **Select one line** (for its length) or **two points** (for the distance between
   them).
2. **Press `K, D`** or activate **Distance Dimension** from the toolbar.
3. **Click to place** the dimension label.
4. **Enter the value** and click OK.

### Horizontal Distance

1. **Select two points** or **one point** (measures from the sketch origin).
2. **Press `L`**.
3. **Enter the horizontal distance** (positive = point to the right of the reference;
   negative = to the left).

### Vertical Distance

1. **Select two points** or **one point**.
2. **Press `I`**.
3. **Enter the vertical distance** (positive = point above the reference; negative
   = below).

!!! tip
    When only one point is selected (no second selection), Horizontal Distance and
    Vertical Distance measure from the sketch origin. This is a quick way to set the
    absolute X or Y coordinate of a vertex.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Value | Distance (mm) | Current measurement | The distance to enforce. Can be negative for signed horizontal/vertical measurements. |
| Mode | Driving / Reference | Driving | See [Driving vs Reference](dimension.md#driving-vs-reference-mode-in-depth). |

---

## Horizontal Distance

### In depth {#horizontal-distance}

Horizontal Distance (`DistanceX`) constrains the X-coordinate difference between
two points. With two points A and B: `B.x - A.x = value`. With one point and the
origin: `point.x = value`.

**Key properties:**
- Sign matters: a negative value places the second point to the *left* of the first.
- Use it instead of a full Distance constraint when you only need the horizontal
  component (leaves the vertical component free to be constrained separately).

## Vertical Distance

### In depth {#vertical-distance}

Vertical Distance (`DistanceY`) constrains the Y-coordinate difference between
two points. With two points: `B.y - A.y = value`. With one point: `point.y = value`.

**Key properties:**
- Positive = second point is above the first.
- Use it to set the height of a feature relative to a baseline edge.

---

## Common mistakes and pitfalls

!!! warning "Distance between two constrained points is redundant"
    **Cause:** Both points were fully fixed by other constraints, so the measured
    distance is already determined. Adding a Distance constraint on top is redundant.  
    **Fix:** Accept the situation (the distance is already set) or redesign the
    constraints so that the Distance constraint is what fixes one of the points.

!!! warning "Negative horizontal distance produces unexpected result"
    **Cause:** You expected `DistanceX = 20` to place a point 20 mm to the right,
    but the *order* of your selection reversed the sign convention.  
    **Fix:** Check which point is the reference and which is the target. If the
    result is mirrored, change the sign: `DistanceX = -20`.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

idx = sketch.addGeometry(
    Part.LineSegment(App.Vector(5, 3, 0), App.Vector(25, 3, 0)), False)

# Fix the line length (straight distance)
sketch.addConstraint(Sketcher.Constraint("Distance", idx, 20.0))

# Fix start point X coordinate (from origin)
sketch.addConstraint(Sketcher.Constraint("DistanceX", idx, 1, -1, 1, 5.0))

# Fix start point Y coordinate (from origin)
sketch.addConstraint(Sketcher.Constraint("DistanceY", idx, 1, -1, 1, 3.0))

doc.recompute()
```

### Method signatures

```python
# Distance on a single line (its length)
sketch.addConstraint(Sketcher.Constraint("Distance", geoId, value))

# Distance between two points
sketch.addConstraint(Sketcher.Constraint("Distance", geo1, vertex1, geo2, vertex2, value))

# Horizontal distance from point to origin
sketch.addConstraint(Sketcher.Constraint("DistanceX", geoId, vertex, -1, 1, value))

# Vertical distance between two points
sketch.addConstraint(Sketcher.Constraint("DistanceY", geo1, vertex1, geo2, vertex2, value))
```

---

## See also

- [Dimension](dimension.md) — unified constraint tool that auto-detects distance type
- [Radius/Diameter](constraint-radius-diameter.md) — constrain circle/arc size
- [Angle](constraint-angle.md) — constrain the angle between lines
- [Lock Position](constraint-lock.md) — set both X and Y at once via the UI
