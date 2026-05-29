# From Part Shape

> **In one sentence:** Tessellate a B-rep solid into a triangulated mesh,
> ready for 3-D printing export or mesh-based analysis.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Create Mesh from Shape…  
**Command:** `Mesh_FromPartShape`  
**Shortcut:** none

Converts a B-rep solid (from Part Design, Part, or other parametric workbenches)
into a `Mesh::Feature` containing a triangulated mesh. This is the standard
workflow for preparing a parametric model for 3-D printing.

---

## Intuition

B-rep geometry is mathematically exact (curved faces, precise edges) but 3-D
printers only understand triangles. From Part Shape samples the B-rep surface
at a controlled density to produce a triangle mesh that approximates the solid.

The quality of the mesh (how closely it matches the true geometry) is controlled
by two parameters: surface deviation and angular deviation.

---

## When to use it

- Preparing a Part Design or Part solid for 3-D printing (export to STL).
- Converting parametric geometry to a mesh for mesh-based analysis.
- Generating a reference mesh for comparison with a scanned part.

## When NOT to use it

- **Don't use it as a final step** — the resulting mesh is a snapshot. If you
  modify the solid, re-run From Part Shape to get a fresh mesh.
- **Don't use it for exact B-rep-to-B-rep conversion** — use File → Export
  with STEP or IGES for that.

---

## Step-by-step

1. Select the solid in the model tree (Part Design Body, Part shape, etc.).
2. Choose **Mesh → Meshes → Create Mesh from Shape…**.
3. The Tessellation dialog opens.
4. Set the tessellation parameters (see below).
5. Click **OK**. A `Mesh::Feature` appears alongside the solid.
6. Export with [Export Mesh](export-mesh.md) → STL.

---

## Parameters

| Parameter | Description | Typical value |
|-----------|-------------|---------------|
| Surface deviation | Max error between mesh and true surface (mm) | 0.1–0.5 mm |
| Angular deviation | Max angle between adjacent triangle normals (°) | 5–10° |
| Relative surface deviation | Express deviation as fraction of bounding box | Off for absolute |
| Apply face colors | Preserve face colours from Part Design | Optional |
| Define segments by face colors | Create mesh regions per colour | Optional |

### Resolution guide

| Use case | Surface deviation | Angular deviation |
|----------|------------------|------------------|
| FDM 3-D printing (0.2 mm layers) | 0.2 mm | 10° |
| SLA/resin printing | 0.05 mm | 5° |
| Visual rendering | 0.5 mm | 15° |
| Fine detail | 0.01 mm | 2° |

---

## Common mistakes and pitfalls

!!! warning "From Part Shape does not link to the solid"
    The generated mesh is a static snapshot. If you modify the parametric
    solid, you must re-run From Part Shape to get an updated mesh.

!!! warning "Very fine tessellation creates huge files"
    Surface deviation of 0.01 mm on a complex part can produce millions of
    triangles — a file too large for most slicers. Use the coarsest setting
    that meets your print resolution.

---

## Python API

```python
import MeshPart
import FreeCAD as App

doc = App.ActiveDocument
body = doc.getObject("Body")

mesh_data = MeshPart.meshFromShape(
    Shape=body.Shape,
    LinearDeflection=0.1,      # surface deviation in mm
    AngularDeflection=0.087,   # 5° in radians
    Relative=False
)

mesh_feature = doc.addObject("Mesh::Feature", "Mesh")
mesh_feature.Mesh = mesh_data
doc.recompute()
```

---

## See also

- [Export Mesh](export-mesh.md) — save the resulting mesh to STL
- [Remesh with Gmsh](remesh-with-gmsh.md) — re-mesh for better element quality
- [Evaluate and Repair](evaluate-and-repair.md) — check the mesh after creation
- [Mesh Workbench](../index.md) — workbench overview
