# Cubic Bézier Curve

> **In one sentence:** Draws a smooth cubic (degree-3) Bézier curve where each
> segment is defined by two endpoints and two visible drag handles.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Bézier Tools → Cubic Bézier Curve  
**Command:** `Draft_CubicBezCurve`

Click to place points alternating endpoint → handle → handle → endpoint.
The tangent at each endpoint is controlled by the adjacent handle, giving
smooth C1-continuous curves across segments.

---

## When to use

Use Cubic Bézier over B-Spline when you need per-segment tangent control
(e.g., logo outlines, organic paths, typography-style curves).

---

## See also

- [Bezier Curve](bezier-curve.md) — arbitrary-degree Bézier
- [B-Spline](bspline.md) — interpolating spline without separate handles
- [Draft Workbench](../index.md) — workbench overview
