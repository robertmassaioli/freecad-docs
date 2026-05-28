# Structured Point Clouds

> **In one sentence:** Triangulate a structured (grid) point cloud directly
> into a mesh by connecting neighbouring grid cells into triangles.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Surface Reconstruction → Structured Point Clouds  
**Shortcut:** none

Converts a `Points::Structured` object (a Width × Height ordered grid of
points) into a triangle mesh by connecting adjacent grid cells. No surface
fitting is performed — the mesh simply links the grid points with edges.
Invalid cells (`NaN` coordinates) result in holes in the mesh.

---

## Intuition

A structured point cloud is like a digital photograph: each pixel has a row
and a column. Structured Point Clouds connects each pixel to its four
neighbours to form two triangles per grid cell, giving you a mesh that is
pixel-accurate — every input point becomes a vertex, and the mesh topology
exactly mirrors the sensor's scan grid.

---

## When to use it

- Your scan comes from a structured sensor (depth camera, structured-light
  scanner, spinning LIDAR) and is already loaded as a `Points::Structured`.
- You want the fastest possible mesh that exactly preserves all scan points.
- You are doing depth-image processing where the grid topology matters.

---

## When NOT to use it

- **Unstructured point clouds** — this tool requires `Points::Structured`
  input. For unstructured clouds, use [Poisson Reconstruction](poisson-reconstruction.md)
  instead.
- **Noisy or sparse sensor data** — directly triangulating a noisy grid
  produces a very rough, bumpy mesh. Consider smoothing the mesh afterwards
  with Mesh workbench tools.

---

## Step-by-step

1. Ensure you have a `Points::Structured` object (created via Points →
   Structured Point Cloud, or imported from a structured format).
2. Select the `Points::Structured` object.
3. Choose **Reverse Engineering → Surface Reconstruction → Structured Point Clouds**.
4. FreeCAD creates a `Mesh::Feature` named "View mesh".

---

## Parameters

This tool has no parameters — the triangulation is fully determined by the
Width × Height grid structure of the input `Points::Structured`.

### NaN handling

Grid cells where the sensor produced no valid return are stored as
`(NaN, NaN, NaN)` in the structured cloud. Any triangle that would include
a `NaN` vertex is skipped, leaving a hole in the mesh at that grid location.

---

## Common mistakes and pitfalls

!!! warning "Tool fails: object is not Points::Structured"
    **Cause:** You selected a plain `Points::Feature` (unstructured cloud)
    instead of a `Points::Structured`.  
    **Fix:** First convert the cloud to structured via Points →
    Structured Point Cloud, then apply this tool.

!!! warning "Resulting mesh is extremely bumpy"
    **Cause:** The raw scan has sensor noise at every grid cell, which the
    direct triangulation faithfully reproduces.  
    **Fix:** Smooth the mesh after triangulation using Mesh workbench →
    Smoothing.

---

## Python API

```python
import ReverseEngineering as Reen
import FreeCAD as App

doc = App.ActiveDocument
structured = doc.getObject("StructuredCloud")  # Points::Structured

mesh_feat = doc.addObject("Mesh::Feature", "ViewMesh")
mesh_feat.Mesh = Reen.viewTriangulation(
    Points=structured.Points,
    Width=structured.Width,
    Height=structured.Height
)
doc.recompute()
```

---

## See also

- [Poisson Reconstruction](poisson-reconstruction.md) — for unstructured clouds or watertight results
- Points workbench — create a `Points::Structured` from an unstructured cloud
- [Reverse Engineering Workbench](../index.md) — workbench overview
