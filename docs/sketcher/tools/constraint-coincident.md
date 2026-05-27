# Coincident Constraint

> **In one sentence:** Force two points to share the same location, effectively
> joining the endpoint of one edge to the endpoint (or midpoint) of another.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher constraints → Coincident Constraint  
**Toolbar:** Sketcher Constraints · **Shortcut:** `C`

Coincident is the most fundamental geometric constraint in Sketcher. It merges two
points into one, ensuring they always occupy the same coordinates. Every closed
profile, every connected chain of lines and arcs, relies on Coincident to form a
single continuous wire rather than a collection of disconnected edges.

In FreeCAD 1.0+, the **Unified Coincident** tool (`C`) handles both the classic
coincident (point-to-point) and the Point on Object case automatically, based on
what you select. The older split commands (`Sketcher_ConstrainCoincident` for
point-to-point, `Sketcher_ConstrainPointOnObject` for point-on-edge) still exist
and are exposed in [Point on Object](constraint-point-on-object.md).

---

## Intuition

Think of two pen strokes that look like they touch on paper — but under a
magnifying glass they are actually separate, with a tiny gap between them. Coincident
is the formal statement "these two points are the same point — not close, not nearly
touching, but *identical*." Once the constraint is applied, no other force can move
them apart.

This matters because the Sketcher solver treats geometry as exact: a near-coincident
vertex still has two degrees of freedom (it can drift). A formally constrained
vertex has zero — it is locked to its partner.

---

## When to use it

- Joining the endpoint of a line to the endpoint of an arc (forming a connected
  chain).
- Centring a circle or arc at a specific point (constrain the circle's centre to
  a vertex or origin point).
- Closing a polyline by joining the last endpoint back to the first.
- After [Mirror Sketch](mirror-sketch.md) or [Carbon Copy](external-geometry.md#carbon-copy),
  connecting the imported geometry to the existing sketch.

## When NOT to use it

- **You want a point to lie somewhere *on* a line or arc but not at an endpoint** —
  use [Point on Object](constraint-point-on-object.md) instead.
- **Two edges should be tangent at their junction** — use
  [Tangent](constraint-tangent.md) in addition to Coincident (Coincident alone does
  not guarantee tangency).

---

## Step-by-step walkthrough

### Point-to-point coincident

1. **Select two points** — click the first vertex, then ++ctrl++-click the second.
   (Alternatively, select both with a box selection.)
2. **Press `C`** or activate **Sketch → Coincident Constraint**.
3. The two points merge to a single location. The solver updates all connected
   geometry.

### Point-to-object (using the unified tool)

1. **Select a point and an edge** — click a vertex, then ++ctrl++-click a line or
   arc.
2. **Press `C`.** The unified Coincident tool detects a point-to-edge selection and
   applies a Point on Object constraint instead.

!!! tip
    You can also apply Coincident by selecting elements first, then pressing `C` —
    the order of selection does not matter for the result.

---

## Parameters

Coincident has no numeric parameters. It is a pure geometric constraint that
removes 2 degrees of freedom (the relative X and Y offset between the two points).

---

### Unified vs split mode in depth

FreeCAD 1.0 introduced the **Unified Coincident** (`Sketcher_ConstrainCoincidentUnified`)
tool, which replaces two older separate commands:

- `Sketcher_ConstrainCoincident` — classic point-to-point
- `Sketcher_ConstrainPointOnObject` — point-to-edge

The unified tool picks the appropriate behaviour based on what you select.

#### Classic Coincident (point-to-point)

Forces two specific vertices to share identical coordinates. Both vertices become
one logical point. This removes exactly 2 DoF.

**Use when:**
- Joining endpoint-to-endpoint (connecting edges end-to-end).
- Centring a circle at a line endpoint.
- Closing a polyline loop.

**Watch out for:** Applying Coincident to two vertices that are both already fully
constrained elsewhere results in a **redundant constraint** (shown in red). Avoid
using Coincident as a substitute for proper structural design — if two points are
already determined to be at the same location by other constraints, an extra
Coincident is redundant.

#### Point on Object

Forces a point to lie on a line, arc, or circle (anywhere along its extent, not
necessarily at an endpoint). This removes 1 DoF (the point can still slide along
the edge). See [Point on Object](constraint-point-on-object.md) for full details.

**Use when:**
- A midpoint or construction point must lie on an edge without being fixed to a
  specific location on that edge.

**Watch out for:** A point constrained to lie on an edge still has 1 remaining DoF
(its position along the edge). Add a second constraint (e.g. a distance or another
Point on Object to a crossing edge) to fix it completely.

---

## Advanced usage

### Centring geometry at the sketch origin

To place a circle's centre at the sketch origin:

1. Select the circle's centre vertex.
2. Select the sketch origin point (the special `ExternalPoint` at coordinate 0, 0).
3. Apply Coincident (`C`).

The origin point is accessible via **Sketch → Select Origin** or by clicking the
central cross in the sketch viewport.

### Multiple coincidents at a node

A single vertex can be coincident with multiple others. For example, a T-junction
(three lines meeting at one point) requires two Coincident constraints: one between
lines A and B, one between A and C. All three lines' endpoints then share the same
location.

---

## Common mistakes and pitfalls

!!! warning "Sketch shows as over-constrained after Coincident"
    **Cause:** Both points were already determined to be at the same location by
    other constraints (e.g. a Lock Position on each), so the Coincident is redundant.  
    **Fix:** Remove one of the Lock Position constraints and let the Coincident
    define the shared location.

!!! warning "Profile appears connected visually but the solver reports DoF"
    **Cause:** Two endpoint vertices appear to touch in the viewport but no Coincident
    constraint exists. The solver sees them as separate free-floating points.  
    **Fix:** Select both vertices and press `C` to apply the Coincident constraint.
    Use [Validate Sketch](validate-sketch.md) to detect all such gaps automatically.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Add two lines
line0 = sketch.addGeometry(
    Part.LineSegment(App.Vector(0, 0, 0), App.Vector(10, 0, 0)), False)
line1 = sketch.addGeometry(
    Part.LineSegment(App.Vector(10, 0, 0), App.Vector(10, 10, 0)), False)

# Make endpoint of line0 coincident with start of line1
sketch.addConstraint(Sketcher.Constraint("Coincident", line0, 2, line1, 1))
doc.recompute()
```

### Method signature

```python
sketch.addConstraint(Sketcher.Constraint(
    "Coincident",
    geo1,    # int — first geometry index
    vertex1, # int — 1 = start point, 2 = end point, 3 = centre/midpoint
    geo2,    # int — second geometry index (-1 = sketch origin, -2 = X-axis, -3 = Y-axis)
    vertex2, # int — same encoding as vertex1
))
```

---

## See also

- [Point on Object](constraint-point-on-object.md) — constrain a point to lie *on* an edge
- [Lock Position](constraint-lock.md) — fix a point to absolute X, Y coordinates
- [Tangent](constraint-tangent.md) — add smooth tangency at a coincident junction
- [Validate Sketch](validate-sketch.md) — find and fix missing coincident constraints
