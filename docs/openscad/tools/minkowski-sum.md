# Minkowski Sum

> **In one sentence:** Compute the Minkowski sum of the selected objects using
> OpenSCAD's CGAL engine — most commonly used to create rounded offsets of shapes.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Minkowski Sum  
**Requires:** OpenSCAD binary installed  
**Shortcut:** none

Passes all selected objects to OpenSCAD as a `minkowski()` call and imports
the result as a `Part` object. All inputs are hidden.

---

## Intuition

The Minkowski sum of shape A and shape B is the set of all points you can
reach by placing shape B at each point of shape A and taking the union.
Practically:

- **Minkowski(solid, sphere of radius r)** = the solid expanded uniformly by
  radius r in all directions, with perfectly rounded corners and edges.
- **Minkowski(shape, small cube)** = the shape expanded with flat-corner
  offsets.

This is one of the most powerful morphological operations in computational
geometry. It can create rounded offsets that are impossible with simple face
offsetting.

---

## When to use it

- You need to expand a solid by a uniform radius with fully rounded convex
  corners (a "blob offset").
- You are computing the swept volume of a tool or end-effector.
- You need a Minkowski sum for CNC clearance analysis.

---

## When NOT to use it

- **For simple face offsets** — use Part → Offset 3D Surface, which is faster
  and produces B-rep output.
- **Without OpenSCAD installed** — requires the OpenSCAD binary.
- **On complex shapes** — Minkowski sum is computationally expensive; it can
  time out on high-polygon-count geometry.

---

## Step-by-step

1. Select two objects (typically the shape to expand and the structuring
   element, e.g. a small sphere).
2. Choose **OpenSCAD → Minkowski Sum**.
3. FreeCAD hides the inputs and creates a `Part` object with the result.

---

## Common mistakes and pitfalls

!!! warning "Operation times out on complex geometry"
    **Cause:** The Minkowski sum algorithm is O(n × m) in the number of
    faces of both inputs. High-resolution meshes make this extremely slow.  
    **Fix:** Reduce mesh resolution before applying. Use Mesh workbench →
    Decimating to reduce polygon count first.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_Minkowski', 0)")
```

---

## See also

- [Hull](hull.md) — convex hull of objects
- [Mesh Boolean](mesh-boolean.md) — includes Minkowski as an operation option
- [OpenSCAD Workbench](../index.md) — workbench overview
