# Approximate Polynomial Surface

> **In one sentence:** Fit a degree-2 Bézier surface patch to a mesh region,
> producing a single quadratic surface that represents planes, cylinders,
> spheres, cones, and saddle shapes.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Approximation → Polynomial Surface  
**Shortcut:** none

Fits a degree-2 polynomial z = f(x, y) to the mesh points and converts
the polynomial coefficients to a 3×3 Bézier control-point grid. Creates a
`Part::Spline` (Geom_BezierSurface) that represents the quadratic approximation.

---

## Intuition

A degree-2 polynomial surface is the simplest possible curved surface — it
can represent all the quadric primitives: planes (all zero), cylinders
(one curved direction), spheres and paraboloids (both directions curved
equally), and saddle shapes (curvatures in opposite directions). One
mathematical formula covers all of them.

Use this when a simple, single-patch surface is sufficient — it is faster
to compute than a full B-spline fit and gives a clean result for regions that
are close to a quadric shape.

---

## When to use it

- The mesh region is approximately planar, cylindrical, spherical, or
  saddle-shaped, and you want a smooth, single-patch surface.
- You need a parametric surface that can be easily edited (the 3×3 control
  grid is simple to manipulate).
- You want a quick approximate surface before deciding whether a full B-spline
  fit is needed.

---

## When NOT to use it

- **Complex curvature** — a degree-2 patch cannot represent sharp bends,
  high-curvature transitions, or regions that require more than one patch.
  Use [Approximate B-Spline Surface](approx-bspline-surface.md) instead.
- **Large regions** — a single quadratic patch cannot accommodate significant
  shape variation across a large area without high residuals.

---

## Step-by-step

1. Isolate a mesh segment of approximately quadric shape using segmentation tools.
2. Select the `Mesh::Feature`.
3. Choose **Reverse Engineering → Approximation → Polynomial Surface**.
4. FreeCAD creates a `Part::Spline` containing the 3×3 Bézier surface.

---

## Parameters

No task panel — the fit is automatic.

---

## Common mistakes and pitfalls

!!! warning "High residual — region is not quadric"
    **Cause:** The mesh has more complex curvature than a degree-2 polynomial
    can represent.  
    **Fix:** Use [Approximate B-Spline Surface](approx-bspline-surface.md)
    with more control points for better accuracy.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Reen_ApproxPolynomial', 0)")
```

---

## See also

- [Approximate B-Spline Surface](approx-bspline-surface.md) — more powerful, handles complex shapes
- [Approximate Plane](approx-plane.md) — analytic plane fit
- [Reverse Engineering Workbench](../index.md) — workbench overview
