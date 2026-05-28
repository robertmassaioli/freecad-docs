# Approximate Cylinder

> **In one sentence:** Fit a best-fit cylinder to a mesh segment and create
> a `Part::Cylinder` aligned to the estimated axis and radius.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Approximation → Cylinder  
**Shortcut:** none

Estimates the cylinder axis from the mesh face normals, then refines the axis
and radius by iterative least-squares fitting. Creates a `Part::Cylinder` with
the fit radius, height spanning the input extent, and placement aligned to the
fit axis.

---

## Intuition

If you have a scan of a bore or a shaft, the scan is a cloud of points on a
cylinder. Approximate Cylinder finds the axis and radius of the cylinder that
best fits those points, in the same way that Approximate Plane finds the best
flat surface. The result is a clean `Part::Cylinder` you can use for
measurement or as a reference feature.

---

## When to use it

- You have a segmented scan of a cylindrical bore, shaft, or pipe and need
  its axis and radius for engineering calculations or further modelling.
- You want to measure the diameter of a scanned cylinder compared to its
  nominal dimension.
- You need the fit cylinder as a reference axis for aligning other features.

---

## When NOT to use it

- **Select only the cylindrical region** — if the mesh includes non-cylindrical
  faces, the fit will be inaccurate. Use [Mesh Segmentation](mesh-segmentation.md)
  to isolate the cylindrical region first.
- **Cone or tapered cylinder** — the tool fits a right circular cylinder only.
  A tapered bore will produce a poor fit with high residual.

---

## Step-by-step

1. Use [Mesh Segmentation](mesh-segmentation.md) or
   [Manual Segmentation](manual-segmentation.md) to isolate the cylindrical
   region as a separate `Mesh::Feature`.
2. Select that `Mesh::Feature`.
3. Choose **Reverse Engineering → Approximation → Cylinder**.
4. FreeCAD creates a `Part::Cylinder`. Check the Report View for the RMS
   residual and fit parameters.

---

## Parameters

No task panel — the fit runs automatically.

### Result properties

| Property | Description |
|----------|-------------|
| Radius | Best-fit cylinder radius |
| Height | Length of the input along the fit axis |
| Placement | Positioned and oriented along the fit axis |

---

## Common mistakes and pitfalls

!!! warning "Fit is inaccurate — non-cylindrical faces included"
    **Cause:** The selected mesh includes flat faces or blending fillets
    adjacent to the cylinder.  
    **Fix:** Isolate just the cylindrical region using segmentation tools
    before fitting.

!!! warning "Fit fails silently (no Part::Cylinder created)"
    **Cause:** The algorithm could not converge — the input may not be
    sufficiently cylindrical.  
    **Fix:** Check that the selected region is approximately cylindrical.
    Try a different segment or use [Approximate B-Spline Surface](approx-bspline-surface.md)
    as a fallback.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Reen_ApproxCylinder', 0)")
```

---

## See also

- [Approximate Plane](approx-plane.md) — fit a plane to a flat segment
- [Approximate Sphere](approx-sphere.md) — fit a sphere to a spherical segment
- [Mesh Segmentation](mesh-segmentation.md) — isolate cylindrical regions first
- [Reverse Engineering Workbench](../index.md) — workbench overview
