# B-Spline

> **In one sentence:** Draw smooth, free-form curves using B-spline mathematics —
> either by placing control points that pull the curve like magnets, or by
> specifying interpolation points that the curve passes through exactly.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → B-Spline  
**Toolbar:** Sketcher Geometries (B-Spline dropdown) · **Shortcut:** `G, B, B` (default: B-Spline by Control Points)

Four tools create B-spline curves, organised along two axes:

|  | **Open (ends free)** | **Periodic (closed loop)** |
|--|---------------------|--------------------------|
| **By control points** | B-Spline | Periodic B-Spline |
| **By interpolation points** | B-Spline by Interpolation | Periodic B-Spline by Interpolation |

- **Control points** pull the curve toward them but the curve does not pass through
  them (except at the endpoints for a non-uniform spline).
- **Interpolation points** are points the curve passes through exactly.
- **Open** splines have a distinct start and end; **Periodic** splines close
  smoothly back on themselves.

---

## Intuition

### Control points vs interpolation

Think of a control-point B-Spline like a flexible ruler bent by a series of
magnets: each magnet pulls the ruler toward it but the ruler never quite touches
the magnet (except at the ends). Moving a magnet changes the curve globally —
all nearby points shift.

An interpolation B-Spline is more like threading a wire through a series of
eyelets: the wire is forced through every eyelet exactly, but it curves smoothly
between them. If you know where the curve *must* pass, use interpolation; if you
want to sculpt the shape by feel, use control points.

### Open vs periodic

An open B-Spline has a start and end point: it can connect to other geometry
(lines, arcs) at those endpoints. A periodic B-Spline forms a seamless closed loop
with no start or end — ideal for completely enclosed profiles like organic outlines
or cam lobe shapes.

---

## When to use it

- You need a smooth, organic curve that cannot be described by a circle or ellipse
  (e.g. an airfoil section, a cam profile, a decorative scroll).
- You are fitting a curve through a set of known data points (interpolation).
- You need a smooth closed outline for a body whose profile is not a standard
  shape (periodic B-Spline).

## When NOT to use it

- **Simple arcs and lines suffice** — B-Splines are harder to constrain and slower
  for the solver. Prefer arcs for round shapes and lines for straight ones.
- **You need an ellipse** — use [Create Circle / Ellipse](create-circle.md); it
  is exact and much easier to constrain.
- **The curve must meet another edge tangentially** — possible with B-Splines but
  fiddly; for smooth joins between an arc and a line, use
  [Fillet](create-fillet.md) or [Tangent](constraint-tangent.md) on simpler
  geometry first.

---

## Step-by-step walkthrough

### B-Spline by Control Points (open)

1. **Press `G, B, B`** or select **B-Spline** from the dropdown.
2. **Click to place control points** one by one. The curve updates in real time,
   pulled toward each control point.
3. **Right-click or press `M`** to finish placing points and exit the tool. The
   spline is open (start and end are distinct).
4. **Constrain the spline:**
    - Fix endpoint positions with [Coincident](constraint-coincident.md) or
      [Lock Position](constraint-lock.md).
    - Constrain the control-point positions (the weight circles) to fix the shape.
    - Use [Tangent](constraint-tangent.md) at endpoints to ensure smooth joins
      with adjacent geometry.

### Periodic B-Spline (closed loop) by Control Points

1. **Select Periodic B-Spline** from the dropdown.
2. **Click control points.** The curve closes automatically after three or more
   points.
3. **Right-click** to finish. There is no start or end — the curve wraps.
4. **Constrain** control point positions to fix the shape.

### B-Spline by Interpolation

1. **Select B-Spline by Interpolation** from the dropdown.
2. **Click interpolation points** — the curve passes through each one exactly.
3. **Right-click** to finish.
4. **Constrain** the interpolation point positions.

### Periodic B-Spline by Interpolation

Same as Interpolation, but closes the curve smoothly back through the first
interpolation point.

---

## Parameters

B-Splines in Sketcher have rich internal geometry:

| Element | Description |
|---------|-------------|
| Control polygon | The dashed polygon connecting control points (visible in edit mode) |
| Control points | The blue squares that pull the curve; each has 2 DoF (X, Y) |
| Knot points | The red squares on the curve body marking knot locations |
| Weight circles | Circles at control points showing relative weight (drag to adjust) |
| Degree | The polynomial degree of each segment (default 3 = cubic) |

The most important parameters to constrain:

| Parameter | Type | Applied via |
|-----------|------|-------------|
| Endpoint position (open spline) | Coordinate | [Coincident](constraint-coincident.md) or [Lock Position](constraint-lock.md) |
| Control point positions | Coordinate | [Lock Position](constraint-lock.md) on each control point |
| Endpoint tangent direction | Angle | [Tangent](constraint-tangent.md) between endpoint and adjacent edge |
| Interpolation point positions | Coordinate | [Coincident](constraint-coincident.md) or [Lock Position](constraint-lock.md) |

---

### B-Spline creation in depth

Each of the four creation modes has qualitatively different behaviour when you
change the shape or add constraints.

#### B-Spline by Control Points (open) (default)

The curve is pulled toward the control points but does not pass through interior
ones. Moving a control point smoothly deforms the whole neighbourhood. Endpoints
*are* interpolated for a standard clamped B-Spline.

**Use this mode when:**

- You want sculpting-style shape control: place magnets and move them until the
  shape looks right.
- You do not care exactly where the curve passes at interior points.

**Watch out for:** Each control point adds 2 DoF. A B-Spline with N control points
needs 2N constraints to be fully constrained (in addition to the endpoint
constraints). Large numbers of unconstrained control points are the most common
cause of B-Spline-related solver slowdowns.

#### Periodic B-Spline by Control Points

A closed loop with no endpoints. The curve wraps smoothly through all control
points. Control points near each other can create a sharp cusp if too close.

**Use this mode when:**

- You need a fully enclosed smooth profile (no start/end seam).
- You are designing a cam lobe, organic enclosure, or decorative profile.

**Watch out for:** It is impossible to apply an endpoint Tangent constraint
(there are no endpoints). To control the curve direction at a specific location,
constrain the positions of the two surrounding control points.

#### B-Spline by Interpolation (open)

The curve passes exactly through every interpolation point. Moving a point moves
the curve at that exact location. Good for fitting a curve to known data points.

**Use this mode when:**

- You know where the curve must pass at specific locations.
- You are reverse-engineering a shape from measurements.

**Watch out for:** Interpolation splines can have higher oscillation between points
(Runge's phenomenon) if the points are unevenly spaced. Use fewer, more evenly
spaced points to get a smoother result.

#### Periodic B-Spline by Interpolation

Closed loop through exact interpolation points. The curve joins the last point
back to the first with full C2 continuity.

**Use this mode when:**

- You need a closed profile and know the exact points it must pass through.

**Watch out for:** The closure point (where the curve wraps back) may have a kink
if the start and end tangents are not consistent. Ensure the first and last
interpolation points are not too close together.

---

## Advanced usage

### Increasing / decreasing degree

Right-click a B-Spline and choose **Increase B-Spline Degree** to add polynomial
degree (smoother, more control) or **Decrease B-Spline Degree** (fewer parameters,
less smooth). Degree 3 (cubic) is standard for most organic profiles.

### Converting a B-Spline to construction

Select the B-Spline and press `G, W` or toggle the Construction button to make it
a reference element. This is useful when the spline is a guide rather than a
profile edge.

### B-Spline weights

Each control point has an associated weight that biases how strongly the curve is
attracted to it. Higher weight pulls the curve closer. Drag the weight circles to
adjust. Weights can be set equal with [Equal](constraint-equal.md) for a uniform
NURBS curve.

---

## Common mistakes and pitfalls

!!! warning "B-Spline has many remaining DoF despite seeming finished"
    **Cause:** Every control point contributes 2 DoF. A 5-point spline has 10 DoF
    from the control points alone, minus whatever has been applied. The solver
    reports the true count.  
    **Fix:** Constrain every control point with [Lock Position](constraint-lock.md)
    or relative-position constraints. Use [Coincident](constraint-coincident.md)
    to anchor endpoints to known vertices.

!!! warning "Solver becomes slow after constraining a B-Spline"
    **Cause:** B-Splines with many control points create large constraint systems.
    Adding dimensional constraints to control points can trigger complex solver
    iterations.  
    **Fix:** Use fewer control points where possible. Consider a lower-degree spline
    or break the curve into shorter segments.

!!! warning "Tangent constraint at endpoint shows as conflicting"
    **Cause:** The endpoint is already fully constrained by a Coincident and a Lock
    Position, and the tangent constraint cannot be satisfied simultaneously.  
    **Fix:** Remove the Lock Position and let the Tangent + Coincident define the
    endpoint's behaviour. Or remove the Tangent and manually align the control
    polygon direction.

---

## Python API

### Minimal example — cubic B-Spline

```python
import FreeCAD as App
import Sketcher
import Part

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Add a simple cubic B-Spline through four control points
poles = [
    App.Vector(0, 0, 0),
    App.Vector(10, 20, 0),
    App.Vector(30, 20, 0),
    App.Vector(40, 0, 0),
]
weights = [1.0, 1.0, 1.0, 1.0]
knots = [0.0, 0.0, 0.0, 0.0, 1.0, 1.0, 1.0, 1.0]  # clamped cubic
mults = [4, 4]
degree = 3
periodic = False

bspline = Part.BSplineCurve()
bspline.buildFromPolesMultsKnots(poles, mults, knots, periodic, degree, weights)

idx = sketch.addGeometry(bspline, False)
print(f"B-Spline added at index {idx}")
doc.recompute()
```

### Key geometry properties

`Part.BSplineCurve` from `sketch.Geometry[idx]`:

| Property | Type | Description |
|----------|------|-------------|
| `NbPoles` | int | Number of control points |
| `NbKnots` | int | Number of knot values |
| `Degree` | int | Polynomial degree |
| `isPeriodic()` | bool | True if the spline is closed |
| `getPoles()` | list | List of control point `App::Vector` positions |

---

## See also

- [Create Arc](create-arc.md) — simpler curved segments
- [Create Line](create-line.md) — straight segments
- [Tangent](constraint-tangent.md) — smooth join at B-Spline endpoint
- [Coincident](constraint-coincident.md) — anchor B-Spline endpoints
- [Trim / Split / Extend](trim-split-extend.md) — modify B-Spline edges
