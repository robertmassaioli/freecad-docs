# B-Spline

> **In one sentence:** Draws a smooth B-spline curve through a set of clicked
> control points, with optional closure into a face.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → B-Spline  
**Command:** `Draft_BSpline`

Each click adds a control point; the curve passes through all points
(interpolating spline). For full NURBS control (weights, custom knots),
use Part workbench NURBS tools.

---

## Key properties

| Property | Description |
|----------|-------------|
| Points | Control point list |
| Closed | Close the spline back to the first point |
| Make Face | Fill a closed spline with a face |
| Parameterization | Controls how knots are spaced |

---

## Common mistakes and pitfalls

!!! warning "Degree increases with point count"
    Draft B-Spline uses a uniform interpolating spline whose effective
    degree grows with the number of control points. For controlled smooth
    curves, use fewer points or the Cubic Bézier tool.

---

## See also

- [Bezier Cubic](bezier-cubic.md) — cubic Bézier with handle control
- [Bezier Curve](bezier-curve.md) — arbitrary-degree Bézier
- [Wire to B-Spline](wire-to-bspline.md) — convert a polyline to a B-spline
- [Draft Workbench](../index.md) — workbench overview
