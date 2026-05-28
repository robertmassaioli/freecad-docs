# Approximate B-Spline Curve

> **In one sentence:** Fit a B-spline curve to the points in a point cloud,
> producing a smooth parametric curve for use as a boundary or guide rail.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Approximation → Approximate B-Spline Curve…  
**Shortcut:** none

Projects all points in the selected `Points::Feature` onto their best-fit
parametric axis (arc-length parameter), then fits a B-spline curve of
specified degree and control-point count. Creates a `Part::Feature` containing
the fitted curve. Supports open and closed curves.

---

## Intuition

Imagine threading a smooth wire through a cloud of points so the wire passes
as close as possible to each point. Approximate B-Spline Curve is that wire —
it finds the smoothest curve consistent with the point data. The degree and
number of control points control how flexible the wire is.

---

## When to use it

- You have a cross-section profile extracted from a scan and need a smooth
  curve from it for use in a Loft or Sweep.
- You want to fit a guide rail curve to point data for use in the Surface
  workbench's [Sections](../../surface/tools/sections.md) tool.
- You need to approximate a scan silhouette as a B-spline for 2-D drawing.

---

## When NOT to use it

- **For a 2-D surface**, use [Approximate B-Spline Surface](approx-bspline-surface.md).
- **For a straight line**, use Draft Line — fitting overhead is unnecessary.

---

## Step-by-step

1. Select a `Points::Feature` representing the curve data.
2. Choose **Reverse Engineering → Approximation → Approximate B-Spline Curve…**.
3. In the task panel, set:
   - **Degree** — 3 (cubic) is standard.
   - **Control points** — more control points = tighter fit.
   - **Closed** — check if the curve should form a loop.
4. Click **OK**. A `Part::Feature` containing the B-spline curve is created.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Degree | Integer | 3 | Polynomial degree. 3 (cubic) is standard. |
| Control points | Integer | 6 | Number of control points. More = tighter fit to data. |
| Continuity | Enum | C2 | Internal continuity: C0, C1, C2. |
| Closed | Boolean | false | Whether the curve forms a closed loop. |

---

## Common mistakes and pitfalls

!!! warning "Curve overshoots at the ends"
    **Cause:** The end control points over-extrapolate beyond the data extent.  
    **Fix:** Reduce the degree or add more control points near the ends.

!!! warning "Closed curve has a kink at the join"
    **Cause:** The join continuity at the closure is C0 if the first and last
    control points don't satisfy the continuity condition.  
    **Fix:** Use C2 continuity and ensure the point cloud actually forms a
    smooth closed loop (the start and end points are close together).

---

## Python API

```python
import ReverseEngineering as Reen
import FreeCAD as App

doc = App.ActiveDocument
pts = doc.getObject("ProfileCloud")

result = doc.addObject("Part::Feature", "BSplineCurve")
result.Shape = Reen.approxCurve(
    Points=pts.Points,
    Degree=3,
    NbPoles=6,
    Continuity="C2",
    Closed=False   # TODO: verify exact API
)
doc.recompute()
```

---

## See also

- [Approximate B-Spline Surface](approx-bspline-surface.md) — 2-D surface fit
- [Approximate Polynomial Surface](approx-polynomial-surface.md) — simple quadric fit
- [Reverse Engineering Workbench](../index.md) — workbench overview
