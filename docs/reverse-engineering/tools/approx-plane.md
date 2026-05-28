# Approximate Plane

> **In one sentence:** Fit a best-fit plane to any geometric object using
> least-squares minimisation and create a `Part::Plane` aligned to the result.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Approximation → Plane  
**Shortcut:** none

Computes the plane that minimises the sum of squared distances from all input
points to the plane (PlaneFit algorithm). Creates a `Part::Plane` positioned
and sized to the bounding extent of the input, oriented with its normal along
the best-fit direction. The RMS residual is reported to the Report View.

---

## Intuition

If you hold a flat piece of card against a bumpy surface, it will touch in
some places and float above others. The best-fit plane is the position of
that card that minimises the total gap, measured in a least-squares sense.
The RMS value tells you how bumpy the surface actually is relative to that
ideal flat card.

---

## When to use it

- You have a scan of a nominally flat face and want to create the reference
  plane that best represents it.
- You need to measure the flatness of a scanned surface (RMS = flatness error).
- You want to use the fit plane as a datum for other modelling operations.

---

## When NOT to use it

- **When the surface is not planar** — the RMS will be high and the plane
  will not be meaningful. Use [Approximate B-Spline Surface](approx-bspline-surface.md)
  for complex shapes.
- **When you need a specific datum plane** (not best-fit) — use the Datum
  Plane tools in Part Design instead.

---

## Step-by-step

1. Select one or more geometric objects (mesh, point cloud, or solid).
2. Choose **Reverse Engineering → Approximation → Plane**.
3. FreeCAD fits a plane to each selected object and creates a `Part::Plane`.
4. Check the **Report View** (View → Panels → Report View) for the RMS value:
   ```
   RMS value for plane fit with N points: X.XXXX
   ```

---

## Parameters

This tool has no task panel — it runs automatically on the selected objects.

### Result properties

| Property | Description |
|----------|-------------|
| Position | Corner of the bounding rectangle on the fit plane |
| Length × Width | Extent of the input projected onto the plane |
| Normal | Best-fit normal direction |

---

## Common mistakes and pitfalls

!!! warning "Plane is in an unexpected orientation"
    **Cause:** The least-squares normal can have two valid orientations
    (pointing either way along the best-fit axis). FreeCAD picks one
    automatically.  
    **Fix:** If the plane needs to face a specific direction, flip its
    placement manually in the Properties panel.

!!! warning "High RMS value — surface is not planar"
    **Cause:** The selected region contains curved or non-planar geometry.  
    **Fix:** Use Mesh Segmentation to isolate the planar region before fitting,
    or check that you selected the right object.

---

## Python API

```python
import ReverseEngineering as Reen
import FreeCAD as App

doc = App.ActiveDocument
mesh = doc.getObject("FlatFaceSegment")

# Fit plane — result is printed to Report View
# The Part::Plane object is added to the document automatically via GUI command
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('Reen_ApproxPlane', 0)")
```

---

## See also

- [Approximate Cylinder](approx-cylinder.md) — fit a cylinder to a mesh segment
- [Approximate B-Spline Surface](approx-bspline-surface.md) — fit free-form NURBS
- [Mesh Segmentation](mesh-segmentation.md) — isolate planar regions first
- [Reverse Engineering Workbench](../index.md) — workbench overview
