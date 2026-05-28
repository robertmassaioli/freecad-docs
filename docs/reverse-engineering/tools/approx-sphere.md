# Approximate Sphere

> **In one sentence:** Fit a best-fit sphere to a mesh segment and create a
> `Part::Sphere` at the estimated centre and radius.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Approximation → Sphere  
**Shortcut:** none

Fits a sphere to the selected mesh using a least-squares algorithm.
Creates a `Part::Sphere` placed at the fit centroid with the best-fit radius.

---

## Intuition

Just as Approximate Plane finds the flattest surface through a cloud of points,
Approximate Sphere finds the spherical surface that all the input points lie
closest to. It computes the centre and radius of that sphere.

---

## When to use it

- You have a scan of a spherical joint, ball bearing, or ball-end feature and
  need its centre position and radius.
- You want to measure the roundness deviation of a scanned sphere.
- You need the sphere centre as a reference point for assembly alignment.

---

## When NOT to use it

- **Hemispheres or partial spheres** — the algorithm works best with more than
  half the sphere covered by the scan. Thin caps may produce unstable fits.
- **Non-spherical regions** — isolate just the spherical segment first.

---

## Step-by-step

1. Isolate the spherical region using [Mesh Segmentation](mesh-segmentation.md).
2. Select the spherical `Mesh::Feature`.
3. Choose **Reverse Engineering → Approximation → Sphere**.
4. FreeCAD creates a `Part::Sphere`.

---

## Parameters

No task panel — the fit runs automatically.

### Result properties

| Property | Description |
|----------|-------------|
| Radius | Best-fit sphere radius |
| Placement | Centre of the fit sphere |

---

## Common mistakes and pitfalls

!!! warning "Sphere centred at wrong location"
    **Cause:** The scan covers less than a hemisphere — the under-constrained
    fit can produce an unstable centre estimate.  
    **Fix:** Ensure at least a hemisphere is included in the scan segment.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Reen_ApproxSphere', 0)")
```

---

## See also

- [Approximate Plane](approx-plane.md) — fit a plane
- [Approximate Cylinder](approx-cylinder.md) — fit a cylinder
- [Mesh Segmentation](mesh-segmentation.md) — isolate the spherical region first
- [Reverse Engineering Workbench](../index.md) — workbench overview
