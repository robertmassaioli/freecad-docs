# Convert to Points

> **In one sentence:** Sample a solid or mesh object at a specified density to
> create a `Points::Feature` point cloud from CAD geometry.

---

## Overview

**Workbench:** Points  
**Menu:** Points → Convert to Points  
**Shortcut:** none

Takes any geometric object — a Part solid, a PartDesign Body, or a mesh —
and produces a `Points::Feature` by sampling points from its surfaces. A
single tolerance parameter controls the maximum spacing between sampled
points.

---

## Intuition

Imagine pressing a sheet of fine mesh against a 3-D surface and marking every
point where the mesh touches the surface. The maximum distance between marks
is the tolerance you set. Convert to Points does exactly this, but
mathematically, producing a cloud that approximates the original surface at
the requested density.

This tool is the bridge from the CAD world (exact B-rep or mesh geometry) to
the point cloud world (unstructured sets of coordinates). You would use it
when another tool or workflow expects a point cloud as its input.

---

## When to use it

- You need a point cloud representation of a CAD part for comparison against
  a scan.
- You want to test a point cloud processing algorithm on known geometry.
- You are using the Reverse Engineering workbench and need a cloud to
  approximate back to surfaces.
- You need to export a Part solid as a cloud (`.ply`, `.pcd`) for a
  downstream tool that does not accept STEP or BREP.

---

## When NOT to use it

- **Don't use this to create a mesh** — use Mesh workbench → From Part Shape
  if you need a triangulated mesh (STL).
- **Don't use this for scan data** — if you already have a cloud from a
  scanner, use [Import Points](import-points.md) instead.

---

## Step-by-step

1. Select one or more geometric objects (Part solids, PartDesign Bodies, or
   mesh features) in the model tree.
2. Choose **Points → Convert to Points**.
3. A dialog appears asking for the **maximum distance** (tolerance in mm).
   Enter the desired point spacing.
4. Click **OK**.
5. FreeCAD creates one `Points::Feature` for each selected object, named
   `<original name> (Points)`.

!!! tip
    Start with a tolerance of 0.1 mm and adjust. For large objects, 1.0 mm
    is usually sufficient for visualisation. For precision comparison with a
    scan, match the tolerance to the scan resolution.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Maximum distance | Length (mm) | 0.1 | Maximum spacing between adjacent sampled points. Smaller = denser cloud, longer computation. |

### Tolerance guide

| Tolerance | Cloud density | Typical use |
|-----------|--------------|-------------|
| 0.01 mm | Very dense | High-accuracy surface comparison |
| 0.1 mm | Dense | General reverse-engineering reference |
| 1.0 mm | Medium | Visualisation, coarse comparison |
| 5.0 mm | Sparse | Quick preview only |

---

## Common mistakes and pitfalls

!!! warning "Very small tolerance causes out-of-memory errors"
    **Cause:** A 0.001 mm tolerance on a 100 mm × 100 mm solid can generate
    10 billion points, exhausting RAM.  
    **Fix:** Start with 0.1 mm and decrease only if needed. Check your
    available RAM before proceeding with sub-0.01 mm tolerances.

!!! warning "Converted cloud has the wrong number of points"
    **Cause:** The sampling algorithm distributes points uniformly across
    the surface area. Highly curved or detailed regions get the same density
    as flat areas.  
    **Fix:** This is expected behaviour. Use Reverse Engineering tools for
    adaptive sampling.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
solid = doc.getObject("Body")   # replace with actual object name

# Convert to points with 0.1 mm tolerance
# Use the GUI command via Gui module (most reliable)
import FreeCADGui as Gui
Gui.Selection.clearSelection()
Gui.Selection.addSelection(solid)
Gui.doCommand("import Points, PointsGui")
Gui.doCommand("PointsGui.convert(App.ActiveDocument.getObject('%s'), 0.1)" % solid.Name)
doc.recompute()
```

---

## See also

- [Import Points](import-points.md) — bring in an existing point cloud file
- [Export Points](export-points.md) — save the resulting cloud to disk
- [Merge Point Clouds](merge-point-clouds.md) — combine clouds from multiple objects
- [Points Workbench](../index.md) — overview of the Points workbench
