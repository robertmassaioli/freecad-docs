# Poisson Reconstruction

> **In one sentence:** Reconstruct a watertight closed mesh from a point cloud
> with per-point normals using the Poisson surface reconstruction algorithm.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Surface Reconstruction → Poisson…  
**Shortcut:** none

Solves a Poisson equation over the space around the point cloud to find an
implicit surface whose gradient matches the input normals, then extracts the
zero-level set as a triangle mesh. The result is always watertight (closed,
no holes) regardless of scan gaps in the input.

---

## Intuition

Imagine the surface you want to reconstruct as an invisible soap bubble.
You have a cloud of points on that bubble, each with a little arrow (normal)
pointing outward. Poisson reconstruction treats those arrows as constraints
and solves a global equation to find the bubble shape that best fits all
the arrows at once. Because it solves globally, it naturally fills in gaps
in the scan — the bubble stays closed even where you have no data.

The trade-off is that it always closes the surface. If you scanned only one
side of an object, Poisson will invent a smooth cap on the unseen side.

---

## When to use it

- You have a dense, well-oriented point cloud with normals and want a clean,
  closed mesh for further modelling.
- Your scan has small gaps (missed regions) that you want filled automatically.
- You need a watertight solid for FEM simulation or 3-D printing from a
  reconstructed scan.

---

## When NOT to use it

- **When your cloud has no normals** — Poisson requires per-point outward
  normals. A plain XYZ cloud will fail or produce garbage. Estimate normals
  in CloudCompare first.
- **For open surfaces** (one side of an object) — Poisson will add an
  invented cap. Use mesh segmentation + B-spline approximation instead.
- **For sharp features** — Poisson smooths everything. Thin walls, sharp
  creases, and fine details are rounded. Use the Screened Poisson variant
  (available in CloudCompare) for better feature preservation.

---

## Step-by-step

1. Import a point cloud with per-point normals into FreeCAD (the normals
   must be pre-computed externally, e.g. in CloudCompare).
2. Select the `Points::Feature` object.
3. Choose **Reverse Engineering → Surface Reconstruction → Poisson…**.
4. In the task panel, set:
   - **Octree depth** — start with 8; increase for finer detail.
   - **Samples per node** — start with 1.0; increase for smoother output.
5. Click **OK**. FreeCAD creates a `Mesh::Feature` of the reconstructed mesh.

!!! tip
    Octree depth 8 works for most mechanical parts. For detailed organic
    shapes or fine surface textures, try depth 10. Above depth 12 the
    computation time grows exponentially.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Octree depth | Integer | 8 | Controls reconstruction resolution. Higher = more detail, longer computation. Typical range: 6–10. |
| Solver divide | Integer | 6 | Depth at which the solver subdivides. Typically octree depth − 2. |
| Samples per node | Float | 1.0 | Minimum sample points per octree cell. Higher = smoother but less detailed. |
| Scale | Float | 1.1 | Ratio of reconstruction cube size to point bounding box size. |

---

## Common mistakes and pitfalls

!!! warning "Reconstruction fails or produces a mess — missing normals"
    **Cause:** The point cloud has no per-point normals, or the normals are
    inconsistently oriented (some pointing inward, some outward).  
    **Fix:** Compute and orient normals in CloudCompare before importing
    (Edit → Normals → Compute, then Edit → Normals → Orient).

!!! warning "Reconstructed mesh adds unwanted caps on open scans"
    **Cause:** Poisson always creates a closed surface. Open scan data gets
    closed with an invented smooth cap.  
    **Fix:** Either trim the cap manually with the Mesh workbench, or use
    B-spline approximation ([Approximate B-Spline Surface](approx-bspline-surface.md))
    for open surfaces.

!!! warning "Fine details are smoothed away"
    **Cause:** Poisson is a global smoothing solver. Sharp edges and thin
    walls are inherently smoothed.  
    **Fix:** Use higher octree depth and lower samples per node. For very
    sharp features, post-process in Meshmixer or use a different algorithm.

---

## Python API

```python
import ReverseEngineering as Reen
import FreeCAD as App

doc = App.ActiveDocument
pts = doc.getObject("PointsWithNormals")   # Points::Feature with normals

mesh_feat = doc.addObject("Mesh::Feature", "PoissonMesh")
# TODO: verify exact Reen API for Poisson
mesh_feat.Mesh = Reen.poissonReconstruction(
    Points=pts.Points,
    OctreeDepth=8,
    SolverDivide=6,
    SamplesPerNode=1.0
)
doc.recompute()
```

---

## See also

- [Structured Point Clouds](structured-point-clouds.md) — direct triangulation for grid scans
- [Mesh Segmentation](mesh-segmentation.md) — segment the reconstructed mesh
- [Approximate B-Spline Surface](approx-bspline-surface.md) — fit NURBS to mesh segments
- [Reverse Engineering Workbench](../index.md) — workbench overview
