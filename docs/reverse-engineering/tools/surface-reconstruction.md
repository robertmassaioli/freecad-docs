# Surface Reconstruction

---

## Poisson Reconstruction {#poisson-reconstruction}

**Menu:** Reverse Engineering → Surface Reconstruction → Poisson…  
**Command:** `Reen_PoissonReconstruction`

Reconstructs a watertight closed mesh surface from a point cloud using the
**Poisson surface reconstruction** algorithm. The input point cloud must have
**per-point normals** — without normals, the algorithm cannot determine surface
orientation and will fail or produce incorrect results.

### How Poisson reconstruction works

The algorithm solves a Poisson equation over the space around the point cloud
to find an implicit function whose gradient matches the input normals. It then
extracts the zero-level set of that function as a triangle mesh. Because it
solves a global equation, the result is always:

- **Watertight** — no holes, even where the scan has gaps.
- **Smooth** — detail at gaps is interpolated.
- **Closed** — a proper manifold solid.

The downside: features not supported by the input normals (sharp edges, thin
walls) are smoothed away. The reconstruction quality is only as good as the
normal accuracy.

### Parameters (task panel)

| Parameter | Description |
|-----------|-------------|
| **Octree depth** | Controls the resolution of the reconstruction. Higher depth → finer detail → longer computation. Typical range: 6–10. |
| **Solver divide** | Depth at which the solver subdivides the problem. Usually set to Octree depth − 2. |
| **Samples per node** | Minimum number of sample points per octree node. Higher values produce smoother but less detailed results. Default: 1.0. |
| **Scale** | Ratio of the reconstruction cube size to the sample bounding box size. Default: 1.1. |

### Prerequisites

- Select a single `Points::Feature` object with per-point normals.
- Normals must be outward-facing (oriented consistently).

If the point cloud has no normals (e.g. imported from a plain `.asc` file
with only X Y Z columns), run a normal estimation step in an external tool
(CloudCompare, Open3D) before using Poisson reconstruction.

### Result

FreeCAD creates a `Mesh::Feature` containing the reconstructed mesh.

---

## Structured Point Clouds {#structured-point-clouds}

**Menu:** Reverse Engineering → Surface Reconstruction → Structured Point Clouds  
**Command:** `Reen_ViewTriangulation`

Triangulates a **structured** point cloud (a `Points::Structured` with Width
and Height properties) directly into a mesh by connecting neighbouring grid
cells into triangles. No surface fitting is performed — the mesh simply
connects the grid points with edges.

### When to use

Use this for data from:
- Depth cameras (RealSense, Azure Kinect, ToF sensors)
- Structured-light scanners that produce ordered row/column grids
- LiDAR sensors in rotary scanner mode

The output is a mesh that matches the scan resolution exactly.

### NaN handling

Grid cells where the scanner produced no return (invalid pixels) are stored
as `NaN` in the `Points::Structured`. The triangulator skips any triangle
that involves a `NaN` vertex, leaving holes in the mesh corresponding to
the scan gaps.

### Step-by-step

1. Select a `Points::Structured` object (created via **Points → Structured
   Point Cloud** or imported directly as a structured format).
2. Go to **Reverse Engineering → Surface Reconstruction → Structured Point Clouds**.
3. FreeCAD creates a `Mesh::Feature` named "View mesh".

### Python API

```python
import ReverseEngineering as Reen
import FreeCAD as App

doc = App.ActiveDocument
structured = doc.getObject("StructuredCloud")

mesh_feat = doc.addObject("Mesh::Feature", "ViewMesh")
mesh_feat.Mesh = Reen.viewTriangulation(
    Points=structured.Points,
    Width=structured.Width,
    Height=structured.Height
)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Poisson requires normals — plain X Y Z fails"
    **Cause:** The Poisson algorithm needs the surface orientation encoded in
    per-point normals. A point cloud with only coordinates has no orientation
    information.  
    **Fix:** Estimate normals externally (CloudCompare: Edit → Normals → Compute)
    before importing the cloud into FreeCAD.

!!! warning "Poisson output is larger than the original scan"
    **Cause:** The Poisson algorithm always closes the surface. If the scan
    is open (e.g. one side of an object), the reconstruction will add a
    smooth cap over the opening.  
    **Fix:** For open objects, use mesh segmentation + B-spline surface
    approximation instead of Poisson reconstruction.

!!! warning "Structured triangulation only works on Points::Structured"
    **Cause:** The command requires a `Points::Structured` object (not a plain
    `Points::Feature`).  
    **Fix:** Convert an unstructured cloud to structured first via **Points →
    Structured Point Cloud**.

---

## See also

- [Segmentation](segmentation.md) — break the reconstructed mesh into surface regions
- [Approximation](approximation.md) — fit analytic surfaces to mesh segments
- Points workbench — import and structure point clouds
