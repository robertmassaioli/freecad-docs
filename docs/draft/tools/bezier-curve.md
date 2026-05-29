# Bézier Curve

> **In one sentence:** Draws a single Bézier curve of arbitrary degree where
> each clicked point adds one more control point.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Bézier Tools → Bézier Curve  
**Command:** `Draft_BezCurve`

The curve degree equals the number of control points minus one:
- 3 points → quadratic (parabola)
- 4 points → cubic
- n points → degree n − 1

For complex curves with many points, the high-degree polynomial becomes
impractical — use [B-Spline](bspline.md) instead.

---

## See also

- [Bezier Cubic](bezier-cubic.md) — cubic Bézier with handle-based editing
- [B-Spline](bspline.md) — piecewise smooth curve for many control points
- [Draft Workbench](../index.md) — workbench overview
