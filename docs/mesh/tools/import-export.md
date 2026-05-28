# Import, Export, and Conversion

> **In one sentence:** These tools move geometry into and out of the Mesh
> workbench — importing mesh files, exporting to 3-D printing formats,
> tessellating solids into meshes, re-meshing with Gmsh, and building
> mesh primitives.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes

| Tool | Description |
|------|-------------|
| [Import Mesh](#import-mesh) | Load STL, OBJ, OFF, PLY, AST files |
| [Export Mesh](#export-mesh) | Save mesh to a file |
| [From Part Shape](#from-part-shape) | Tessellate a solid into a mesh |
| [Remesh with Gmsh](#remesh-with-gmsh) | Re-mesh using the Gmsh mesher |
| [Build Regular Solid](#build-regular-solid) | Create a mesh primitive shape |

---

## Import Mesh

**Menu:** Mesh → Meshes → Import  
**Command:** `Mesh_Import`

Loads a mesh file from disk and adds it as a `Mesh::Feature` object in
the model tree.

### Supported file formats

| Format | Extension | Description |
|--------|-----------|-------------|
| STL | .stl, .ast | Stereolithography — 3-D printing standard |
| OBJ | .obj | Wavefront Object — common in 3-D graphics |
| OFF | .off | Object File Format — simple triangle list |
| PLY | .ply | Polygon File Format — scan data standard |
| SMESH | .smesh | SMESH mesh format |
| NASTRAN | .nas, .bdf | NASTRAN bulk data mesh |
| Abaqus | .inp | Abaqus input file (mesh section) |

STL is the most common format for 3-D printing. PLY is common for
photogrammetry and LiDAR point clouds (when converted to a mesh).

### After import

The imported mesh appears as a grey surface in the 3-D view. Check it
immediately with **Analyse → Evaluate and Repair** to find defects.

### Python API

```python
import Mesh

Mesh.insert("/path/to/model.stl", App.ActiveDocument.Name)
App.ActiveDocument.recompute()
```

---

## Export Mesh

**Menu:** Mesh → Meshes → Export  
**Command:** `Mesh_Export`

Saves the selected mesh to a file. The file format is determined by the
extension chosen in the save dialog.

### Export format considerations

| Format | Best for |
|--------|---------|
| STL (binary) | 3-D printing — compact binary format |
| STL (ASCII) | Debugging — human-readable but large |
| OBJ | Import into 3-D graphics tools (Blender, etc.) |
| PLY (binary) | Point cloud / scan data tools |
| OFF | Simple geometry exchange |

**Binary STL** is the preferred format for 3-D printing — it is 5–10×
smaller than ASCII STL for the same mesh.

### Python API

```python
import Mesh

mesh = App.ActiveDocument.getObject("Mesh")
Mesh.export([mesh], "/path/to/output.stl")
```

---

## From Part Shape

**Menu:** Mesh → Meshes → Create Mesh from Shape  
**Command:** `Mesh_FromPartShape`

Converts a B-rep solid (from Part Design, Part, or other parametric
workbenches) into a triangulated mesh. This is the standard way to
prepare a model for 3-D printing export.

### Step-by-step

1. Select a solid in the model tree (Part Design Body, Part Shape, etc.).
2. Choose **Mesh → Meshes → Create Mesh from Shape**.
3. The Tessellation dialog opens.
4. Set the mesh parameters (see below).
5. Click **OK**. A `Mesh::Feature` appears in the model tree alongside
   the solid.
6. Export the mesh to STL with **Mesh → Meshes → Export**.

### Tessellation parameters

| Parameter | Description | Typical value |
|-----------|-------------|---------------|
| Surface deviation | Maximum error between mesh and true surface (mm) | 0.1–0.5 mm |
| Angular deviation | Maximum angle between adjacent triangle normals (°) | 5–10° |
| Relative surface deviation | Express surface deviation as fraction of bounding box | Off for absolute |
| Apply face colors | Preserve face colours from Part Design | Optional |
| Define segments by face colors | Create separate mesh regions per colour | Optional |

**Resolution guide:**

| Use case | Surface deviation | Angular deviation |
|----------|------------------|------------------|
| FDM 3-D printing (0.2 mm layers) | 0.2 mm | 10° |
| SLA/resin printing | 0.05 mm | 5° |
| Visual rendering | 0.5 mm | 15° |
| Fine detail machining | 0.01 mm | 2° |

Lower values → more triangles → larger file and slower slicing. For
typical FDM printing, 0.1 mm / 5° is a good starting point.

### Python API

```python
import MeshPart

doc = App.ActiveDocument
body = doc.getObject("Body")

# Tessellate the solid
mesh_data = MeshPart.meshFromShape(
    Shape=body.Shape,
    LinearDeflection=0.1,
    AngularDeflection=0.087,   # 5° in radians
    Relative=False
)

mesh_feature = doc.addObject("Mesh::Feature", "Mesh")
mesh_feature.Mesh = mesh_data
doc.recompute()
```

---

## Remesh with Gmsh

**Menu:** Mesh → Meshes → Remesh by Gmsh  
**Command:** `Mesh_RemeshGmsh`

Re-meshes an existing mesh using the external Gmsh mesher. Gmsh produces
a higher-quality mesh than FreeCAD's built-in tessellator — better element
shape, more uniform triangle sizes, and controlled element size.

### When to use Gmsh re-meshing

- The imported STL has poor triangle quality (very flat or elongated
  triangles).
- You need uniform element size for FEM analysis from a mesh source.
- You want to reduce the triangle count while maintaining surface accuracy.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Max element size | Largest triangle edge length (mm) |
| Min element size | Smallest triangle edge length (mm) |
| Angle tolerance | Curvature-driven size reduction at curved regions |

Gmsh must be installed and its path set in **Edit → Preferences → Mesh → Gmsh**.

---

## Build Regular Solid

**Menu:** Mesh → Meshes → Build Solid  
**Command:** `Mesh_BuildRegularSolid`

Opens the Regular Solid dialog to create a mesh primitive directly in
the Mesh workbench — without needing a B-rep solid first.

### Available primitives

| Primitive | Parameters |
|-----------|-----------|
| Cube | Length, Width, Height |
| Cylinder | Radius, Length, Closed, Edge angle, Number of edges |
| Cone | Radius 1, Radius 2, Length, Closed, Edge angle, Edges |
| Sphere | Radius, Edge angle, Number of edges (lat/lon) |
| Ellipsoid | Radius 1, Radius 2, Edge angle, Edges |
| Torus | Radius 1, Radius 2, Edge angle, Edges |

These are mesh approximations of the primitives — not exact B-rep
geometry. For engineering use, build the primitive in the Part workbench
and use **From Part Shape** to convert.

---

## Common mistakes and pitfalls

!!! warning "Mesh units are always millimetres"
    FreeCAD's internal unit is millimetres. When importing an STL from
    a tool that exports in inches (some US-based CAD tools), the mesh
    will appear 25.4× too small. Scale the mesh by 25.4 after import
    with **Mesh → Scale**.

!!! warning "From Part Shape does not link to the solid"
    The generated mesh is a snapshot — it does not update if the solid
    changes shape. If you modify the Part Design model, re-run
    **From Part Shape** to get a fresh mesh.

!!! warning "Very fine tessellation parameters create huge files"
    A surface deviation of 0.01 mm on a complex part can produce millions
    of triangles — a 100 MB STL file that most slicers struggle with.
    Use the coarsest resolution that meets your printer's resolution.

!!! warning "ASCII STL is 5× larger than binary STL"
    Some tools export ASCII STL by default. Choose binary STL for export
    to 3-D printing. Both are identical in geometry — binary is just
    a more compact encoding.

---

## See also

- [Analyse](analyse.md) — check imported meshes for defects
- [Modify](modify.md) — repair, smooth, and decimate meshes
- [Boolean and Cutting](boolean-cutting.md) — cut or combine meshes
