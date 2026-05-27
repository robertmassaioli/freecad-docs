# Tangent Constraint

> **In one sentence:** Force a line and a curve (or two curves) to touch smoothly
> at a shared point, with no visible corner or kink at the junction.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Tangent/Collinear Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `T`

Tangent removes 1 degree of freedom — the relative angle at the junction between
two edges. When applied between a line and an arc (or two arcs), it forces the
edges to share the same tangent direction at their meeting point, eliminating the
angle discontinuity (kink).

When applied to two *lines* that share a point, Tangent enforces collinearity
(they become part of the same straight line). This is noted in the UI as
"Tangent/Collinear".

---

## Intuition

Imagine two railway tracks — a straight section and a curved section — meeting at
a junction. If the curve does not align with the straight section at the join point,
there is a visible kink and trains would feel a jolt. Tangent removes that jolt by
forcing the curved track to arrive at exactly the same angle as the straight track
leaves it.

For a line-to-arc junction this means the line is tangent to the circle (the line
touches the circle at exactly one point and is perpendicular to the radius at that
point). For arc-to-arc it means the two circles share the same tangent line at the
meeting point.

---

## When to use it

- You are drawing a blended outline (straight sections transitioning smoothly into
  arcs) and need smooth, G1-continuous joins.
- You want a circle to be tangent to a line (the classic tangent-line construction).
- You drew a Polyline with the arc mode toggled on (`M` key) and want to ensure the
  auto-generated Tangent constraints are still correct after editing.
- You need two arcs to blend smoothly (e.g. an S-curve).

## When NOT to use it

- **You just want the edges to meet without a smooth join** — use
  [Coincident](constraint-coincident.md) alone at the junction.
- **You want two lines to form one straight line** — use
  [Collinear](constraint-tangent.md) (same constraint, different selection: two
  lines give collinearity).
- **You need a circle centred on a line** — use
  [Point on Object](constraint-point-on-object.md) for the centre.

---

## Step-by-step walkthrough

1. **Select two adjacent edges** (a line and an arc, two arcs, or two lines).
   For a line-arc join, click near the shared endpoint for the best result.
2. **Press `T`** or activate **Sketch → Tangent/Collinear Constraint**.
3. The edges adjust to be tangent at their meeting point. If they were not already
   connected, add [Coincident](constraint-coincident.md) to join the endpoints.

!!! tip
    The Polyline tool (with `M` toggled) applies Tangent automatically at each
    line-to-arc junction. Manual Tangent is needed when you draw the elements
    separately.

---

## Parameters

No numeric parameters. Removes 1 DoF.

---

## Advanced usage

### Tangent at a specific point

If two curves share a common point but you want to specify *which* point is the
tangency point (e.g. two arcs that cross at multiple locations), select the
shared endpoint explicitly before applying Tangent.

### Collinear lines

When Tangent is applied to two lines that share an endpoint, it forces them to
be collinear — they become two segments of the same infinite line. This is useful
for modelling a line interrupted by a boss or slot: draw the two segments, join
their adjacent endpoints with Coincident, and then apply Tangent to make them
perfectly collinear.

---

## Common mistakes and pitfalls

!!! warning "Tangent applied but kink still visible"
    **Cause:** The two edges are tangent but their shared endpoint is not formally
    Coincident — they look joined but are not. The Tangent constraint only aligns
    directions, not positions.  
    **Fix:** Add [Coincident](constraint-coincident.md) between the shared endpoints.

!!! warning "Tangent makes the sketch over-constrained"
    **Cause:** The angle at the junction was already forced by other constraints
    (e.g. both a horizontal line and a circle with a coincident centre imply a
    specific tangent at their meeting point).  
    **Fix:** Check if tangency is already implied; if so, the extra Tangent is
    redundant and should be removed.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# A line ending at (10, 0) and an arc starting there
line = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(10, 0, 0)), False)

import math
arc = sketch.addGeometry(
    Part.ArcOfCircle(
        Part.Circle(App.Vector(10, 5, 0), App.Vector(0, 0, 1), 5),
        -math.pi / 2, 0), False)

# Join them
sketch.addConstraint(Sketcher.Constraint("Coincident", line, 2, arc, 1))
# Make them tangent
sketch.addConstraint(Sketcher.Constraint("Tangent", line, arc))
doc.recompute()
```

---

## See also

- [Coincident](constraint-coincident.md) — join endpoints before applying Tangent
- [Perpendicular](constraint-perpendicular.md) — 90° angle at a junction
- [Create Fillet](create-fillet.md) — automatically add a tangent arc between two lines
- [Create Arc](create-arc.md) — draw arcs to blend with lines
