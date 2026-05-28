# Segmentation

> **In one sentence:** Segmentation tools split a mesh into meaningful
> sub-regions — by connectivity (Merge, Split Components), by surface
> curvature, or by fitting geometric primitives — making it easier to
> analyse, repair, or reconstruct individual surfaces.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes

| Tool | Description |
|------|-------------|
| [Merge](#merge) | Merge two or more meshes into one mesh object |
| [Split Components](#split-components) | Split a mesh into its disconnected components |
| [Segmentation](#segmentation-curvature) | Segment the mesh by curvature into regions |
| [Segmentation — Best Fit](#segmentation-best-fit) | Segment by fitting geometric primitives |

---

## Intuition

### What is mesh segmentation?

Segmentation divides a mesh into labelled regions based on some criterion.
In FreeCAD, the two segmentation approaches are:

1. **Connectivity** — split at existing disconnected components (Merge /
   Split Components).
2. **Surface type** — group triangles that belong to the same type of
   geometric surface (plane, cylinder, sphere, cone, torus) — the
   Best Fit tool.

### Use cases for segmentation

- **Reverse engineering:** Segment an imported scan into individual
  surfaces (plane, cylinder, etc.), then fit exact geometric primitives
  to each region for reconstruction as a B-rep solid.
- **Repair:** Identify and isolate specific regions of the mesh for
  targeted repair.
- **Multi-material printing:** Split a mesh into separate components for
  different print materials or colours.
- **Analysis:** Isolate a region of interest for curvature or inspection.

---

## Merge

**Menu:** Mesh → Meshes → Merge  
**Command:** `Mesh_Merge`

Combines two or more separate `Mesh::Feature` objects into a single
mesh object. The meshes are merged into one data structure — they need
not be touching or overlapping.

### Merge vs Union

| | Merge | Union |
|--|-------|-------|
| Combines geometry | Yes | Yes |
| Computes topology | No — appends triangles | Yes — resolves intersections |
| Requires solid meshes | No | Yes |
| Result | Single mesh object (may be multi-component) | Single manifold solid mesh |
| Speed | Instant | Can be slow |

Use **Merge** to combine separate mesh objects into one object for
organization. Use **Union** (Boolean) when you need a single fused solid.

### Step-by-step

1. Select all meshes to merge (hold Ctrl to multi-select).
2. Choose **Mesh → Meshes → Merge**.
3. A new mesh object appears containing all triangles from all inputs.
4. The original mesh objects are retained.

### Python API

```python
import Mesh

m1 = App.ActiveDocument.getObject("Mesh")
m2 = App.ActiveDocument.getObject("Mesh001")

combined = m1.Mesh.copy()
combined.merge(m2.Mesh)

merged = App.ActiveDocument.addObject("Mesh::Feature", "Merged")
merged.Mesh = combined
App.ActiveDocument.recompute()
```

---

## Split Components

**Menu:** Mesh → Meshes → Split by components  
**Command:** `Mesh_SplitComponents`

Splits a mesh that contains multiple disconnected components into separate
`Mesh::Feature` objects — one object per connected component.

### When to use

- After a boolean difference that leaves disconnected fragments.
- After importing a mesh that contains multiple objects as one file.
- To isolate the main body from noise fragments for targeted cleanup.

### Workflow with Remove Components

```
1. Import mesh → one mesh object with many components
2. Mesh_SplitComponents → separate objects per component
3. Delete small noise objects in the model tree
4. Mesh_Merge → recombine the remaining objects
```

This gives precise control over which components to keep, compared to the
threshold-based **Remove Components** tool.

### Python API

```python
import Mesh

mesh = App.ActiveDocument.getObject("Mesh")
components = mesh.Mesh.getSeparateComponents()

for i, comp in enumerate(components):
    feature = App.ActiveDocument.addObject("Mesh::Feature", f"Component_{i}")
    feature.Mesh = comp

App.ActiveDocument.recompute()
```

---

## Segmentation (Curvature) {#segmentation-curvature}

**Menu:** Mesh → Meshes → Segmentation  
**Command:** `Mesh_Segmentation`

Segments the mesh by grouping adjacent triangles with similar surface
normal directions and curvature properties. The result is a set of
named mesh regions, each stored as a separate component in the mesh
or as a segmentation object.

### Segmentation parameters

| Parameter | Description |
|-----------|-------------|
| Smooth mesh | Pre-smooth before segmenting to reduce noise effects |
| Plane | Detect flat regions (low curvature) |
| Max plane curvature | Maximum curvature to classify a triangle as flat |
| Cylinder | Detect cylindrical regions |
| Min cylinder radius | Minimum cylinder radius to detect |
| Max cylinder radius | Maximum cylinder radius to detect |
| Sphere | Detect spherical regions |
| Min sphere radius | Minimum sphere radius to detect |
| Max sphere radius | Maximum sphere radius to detect |
| Freeform | Classify remaining triangles as freeform (non-primitive) |
| Max curvature | Maximum curvature for freeform classification |

### Reading the result

Each detected region is coloured distinctly in the 3-D view. Regions
that were classified as planes, cylinders, or spheres are labelled
accordingly. Unclassified regions appear in a neutral colour (freeform).

---

## Segmentation — Best Fit

**Menu:** Mesh → Meshes → Segmentation — Best Fit  
**Command:** `Mesh_SegmentationBestFit`

A more powerful version of Segmentation that not only classifies regions
but also **fits the optimal geometric primitive** to each classified
region and reports the fit quality.

### Supported primitives

| Primitive | Parameters computed |
|-----------|-------------------|
| Plane | Normal vector, D (distance from origin), RMS fit error |
| Cylinder | Axis direction, radius, RMS fit error |
| Sphere | Centre point, radius, RMS fit error |

### Workflow for reverse engineering

```
1. Import STL scan mesh
2. Evaluate & Repair — fix defects
3. Smooth — reduce scan noise
4. Segmentation Best Fit — classify surfaces
5. Review the fitted primitives (axis, radius, etc.)
6. Reconstruct: use the fitted parameters to create exact B-rep
   primitives in Part workbench
```

For example, if Best Fit reports a cylindrical region with radius 12.5 mm
and axis direction (0, 0, 1), create a Part cylinder with radius = 12.5 mm
as the reconstructed exact geometry.

### Fit quality

The **RMS fit error** (root mean square deviation of mesh vertices from
the fitted primitive) measures how well the segment matches the ideal shape:

- RMS < 0.1 mm: excellent fit — the segment is well-described by this primitive.
- RMS 0.1–0.5 mm: acceptable for rough reconstruction.
- RMS > 0.5 mm: poor fit — the segment may not be the assumed primitive type,
  or the mesh resolution is too coarse.

---

## Common mistakes and pitfalls

!!! warning "Split Components creates many objects for meshes with noise"
    An imported scan often has dozens or hundreds of disconnected noise
    fragments. Split Components will create that many separate objects in
    the model tree. Use **Remove Components** with a size threshold first
    to eliminate the noise before splitting.

!!! warning "Segmentation is sensitive to mesh quality"
    Rough, noisy meshes produce poor segmentation results — smooth the
    mesh before segmenting (Mesh → Smoothing) and use a generous
    curvature tolerance for planes and cylinders.

!!! warning "Best Fit requires a pre-classified segment"
    Segmentation Best Fit works on segments that were first classified by
    the Segmentation tool. Run **Segmentation** first to identify the
    region types, then use Best Fit to compute the primitive parameters.

!!! warning "Merge does not fuse the meshes"
    After Merge, the combined mesh object may contain multiple disconnected
    components. It is not a manifold solid. If you need a fused solid, use
    **Boolean → Union** after Merge.

---

## See also

- [Modify](modify.md) — remove components and repair before segmenting
- [Analyse](analyse.md) — evaluate mesh quality
- [Boolean and Cutting](boolean-cutting.md) — split meshes at cuts before segmenting
