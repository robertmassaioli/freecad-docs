# Approximate B-Spline Surface

> **In one sentence:** Fit a free-form NURBS surface to a point cloud or mesh
> with adjustable degree, control-point density, and smoothing.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Approximation → Approximate B-Spline Surface…  
**Shortcut:** none

The most capable approximation tool in the workbench. Constructs a NURBS
surface of user-specified degree and control-point count that minimises the
distance from the surface to all input points, with a smoothing weight to
trade off accuracy against fairness. The result is a `Part::Spline` object.

---

## Intuition

A B-spline surface is like a sheet of flexible plastic controlled by a grid
of "handles" (control points). Pulling a handle bends the sheet; adding more
handles gives finer control. Approximate B-Spline Surface places those handles
automatically to make the sheet conform as closely as possible to your point
cloud, while the smoothing parameter prevents the sheet from following every
bit of noise.

---

## When to use it

- You have a complex freeform mesh (car body, consumer product, organic shape)
  that cannot be described by analytic primitives.
- You want a single parametric NURBS surface for subsequent use in Part or
  Surface workbench tools.
- You need precise control over the accuracy/smoothness trade-off.

---

## When NOT to use it

- **Simple quadric regions** — use [Approximate Polynomial Surface](approx-polynomial-surface.md)
  for planes, cylinders, and spheres; it is simpler and faster.
- **Very large meshes with millions of faces** — computation time grows with
  point count. Downsample the mesh first with Mesh workbench → Decimating.

---

## Step-by-step

1. Select a `Points::Feature` (cloud) or `Mesh::Feature`.
2. Choose **Reverse Engineering → Approximation → Approximate B-Spline Surface…**.
3. In the task panel, set:
   - **U degree** and **V degree** — start with 3 (cubic).
   - **U control points** and **V control points** — start with 6×6; increase
     for more detail.
   - **Iterations** — 3–5 for most cases.
   - **Smoothing** — higher values give a fairer surface but less accuracy.
4. Click **OK**. A `Part::Spline` is created.

!!! tip
    Use the minimum control-point count that gives an acceptable visual result.
    Too many control points with low smoothing causes oscillation (Runge's
    phenomenon) — the surface ripples between data points.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| U degree | Integer | 3 | Polynomial degree in U direction. 3 (cubic) is standard; higher is rarely needed. |
| V degree | Integer | 3 | Polynomial degree in V direction. |
| U control points | Integer | 6 | Number of control points in U. More = finer detail. |
| V control points | Integer | 6 | Number of control points in V. |
| Iterations | Integer | 3 | Refinement iterations. More = closer to data. |
| Smoothing | Float | 0.1 | Smoothing weight. 0 = pure fit; higher = fairer surface. |
| Continuity | Enum | C2 | Internal NURBS continuity: C0, C1, C2. |

### Parameter guidelines

| Goal | Settings |
|------|---------|
| Smooth result, small deviations acceptable | High smoothing (0.5–1.0), moderate control points |
| Accurate fit, data is clean | Low smoothing (0.01–0.1), moderate control points |
| Avoid oscillation | Use degree 3, start with few control points and increase |
| Large region, complex shape | Increase U and V control points incrementally |

---

## Common mistakes and pitfalls

!!! warning "Surface oscillates / ripples between data points"
    **Cause:** Too many control points with too little smoothing — the solver
    over-fits the data including noise.  
    **Fix:** Reduce control points or increase smoothing. Degree 3 cubic is
    almost always better than degree 5+ for mechanical parts.

!!! warning "Surface is too smooth and misses sharp features"
    **Cause:** Smoothing weight is too high.  
    **Fix:** Reduce smoothing weight. For sharp feature preservation, consider
    post-processing with CAD tools (Part → Fillet/Chamfer).

---

## Python API

```python
import ReverseEngineering as Reen
import FreeCAD as App

doc = App.ActiveDocument
pts = doc.getObject("ScanCloud")   # Points::Feature or Mesh::Feature

result = doc.addObject("Part::Spline", "BSurface")
result.Shape = Reen.approxSurface(
    Points=pts.Points,
    UDegree=3,
    VDegree=3,
    NbUPoles=6,
    NbVPoles=6,
    Iterations=3,
    Smoothing=0.1,
    Continuity="C2"   # TODO: verify exact parameter names
)
doc.recompute()
```

---

## See also

- [Approximate B-Spline Curve](approx-bspline-curve.md) — fit a curve instead of a surface
- [Approximate Polynomial Surface](approx-polynomial-surface.md) — simpler, for quadric regions
- [Mesh Segmentation](mesh-segmentation.md) — isolate regions before fitting
- [Reverse Engineering Workbench](../index.md) — workbench overview
