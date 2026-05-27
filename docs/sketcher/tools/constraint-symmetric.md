# Symmetric Constraint

> **In one sentence:** Force two points (or two edges) to be mirror images of
> each other about a symmetry axis — a line, a point, or one of the sketch axes.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Symmetric Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `S`

Symmetric removes 1 DoF by constraining the midpoint of the line joining two
selected points to lie on the axis, *and* the joining line to be perpendicular to
the axis. This ensures perfect bilateral symmetry.

---

## Intuition

Think of folding a piece of paper along a crease line: whatever is on the left
side is exactly mirrored on the right. Symmetric encodes that mirror relationship
as a constraint — you can move either point or the axis, and the symmetry is
maintained automatically.

---

## When to use it

- You have a symmetric profile and want to draw only one half and let the other
  half follow automatically.
- You need the centre of a feature (e.g. a slot) to be symmetric about the
  sketch Y-axis.
- You are positioning two identical holes equidistant from the centre.
- Ensuring a rectangular profile is centred on the origin (apply Symmetric to
  opposite corners with the origin as the axis point).

## When NOT to use it

- **You want a mirrored copy as independent geometry** — use
  [Mirror Sketch](mirror-sketch.md) or [Symmetry (transform)](symmetry.md) instead.
- **The symmetry axis should be a new line** — draw the line first, then apply
  Symmetric.

---

## Step-by-step walkthrough

### Symmetric about a line

1. **Select** the first point, the second point, and the symmetry line
   (++ctrl++-click each in sequence, line last).
2. **Press `S`** or activate **Sketch → Symmetric Constraint**.
3. The two points move to be symmetric about the line.

### Symmetric about a point

1. **Select** the first point, the second point, and the symmetry point (midpoint).
2. **Press `S`.** The two outer points are mirrored through the centre point.

!!! tip
    To make a sketch symmetric about the sketch Y-axis, select the two points and
    then select the Y-axis line (the vertical construction line through the origin).

---

## Parameters

No numeric parameters. Removes 1 DoF.

---

## Common mistakes and pitfalls

!!! warning "Three elements must be selected in the correct order"
    **Cause:** Symmetric requires exactly 3 selections: point 1, point 2, and the
    axis (line or point). Selecting in the wrong order or selecting only 2 elements
    will not apply the constraint.  
    **Fix:** Deselect all, then click point 1, ++ctrl++-click point 2,
    ++ctrl++-click the axis.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Two points symmetric about the Y-axis (geoId -2 = Y-axis)
pt0 = sketch.addGeometry(Part.Point(App.Vector(-10, 5, 0)), False)
pt1 = sketch.addGeometry(Part.Point(App.Vector(10, 5, 0)), False)

# Symmetric about the sketch Y-axis
sketch.addConstraint(Sketcher.Constraint("Symmetric", pt0, 1, pt1, 1, -2, 1))
doc.recompute()
```

### Method signature

```python
sketch.addConstraint(Sketcher.Constraint(
    "Symmetric",
    geo1,    # int — first element
    vertex1, # int — vertex on first element (1=start, 2=end, 3=centre)
    geo2,    # int — second element
    vertex2, # int — vertex on second element
    geoSym,  # int — symmetry axis geometry id (-2=Y-axis, -3=X-axis, or a line)
    vertexSym, # int — vertex on symmetry axis (3 = midpoint/any point for a line)
))
```

---

## See also

- [Mirror (Symmetry transform)](symmetry.md) — create a mirrored copy of geometry
- [Equal](constraint-equal.md) — force equal size (often used alongside Symmetric)
- [Coincident](constraint-coincident.md) — fix the midpoint of a symmetric pair to the axis
- [Horizontal/Vertical](constraint-horizontal-vertical.md) — often combined with Symmetric for axis-aligned profiles
