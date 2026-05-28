# Segmentation

Segmentation tools split a mesh into labelled regions — either automatically
based on surface curvature, manually by painting, or by the mesh's existing
disconnected components.

---

## Mesh Segmentation… {#mesh-segmentation}

**Menu:** Reverse Engineering → Segmentation → Mesh Segmentation…  
**Command:** `Reen_Segmentation`

Automatically segments a mesh into regions based on surface type using
curvature analysis. Each region is classified as one of:

| Surface type | Curvature signature |
|-------------|---------------------|
| Plane | Both principal curvatures ≈ 0 |
| Cylinder | One curvature ≈ 0, one curvature > 0 |
| Sphere | Both curvatures equal and > 0 |
| Free-form | Everything else |

The segmentation runs as a region-growing algorithm seeded from faces with
low classification uncertainty.

### Task panel parameters

| Parameter | Description |
|-----------|-------------|
| **Smoothing steps** | Number of Laplacian smoothing passes applied before curvature estimation. More smoothing reduces noise but blurs sharp transitions. |
| **Plane tolerance** | Maximum curvature magnitude for a face to be classified as planar. |
| **Cylinder tolerance** | Curvature asymmetry tolerance for cylinder classification. |
| **Sphere tolerance** | Maximum deviation from equal-curvature for sphere classification. |
| **Min region size** | Regions with fewer faces than this threshold are discarded. |

### Step-by-step

1. Select a single `Mesh::Feature`.
2. Go to **Reverse Engineering → Segmentation → Mesh Segmentation…**.
3. Adjust tolerances and smoothing in the task panel.
4. Click **OK**. FreeCAD creates a group named "Segments" with one
   `Mesh::Feature` per identified region.

### Result

Each segment is a sub-mesh corresponding to one surface type. Use the
[Approximation](approximation.md) tools to fit a `Part` primitive or
B-spline surface to each segment.

---

## Manual Segmentation… {#manual-segmentation}

**Menu:** Reverse Engineering → Segmentation → Manual Segmentation…  
**Command:** `Reen_SegmentationManual`

Opens an interactive task panel where you paint mesh faces by region using
a brush. Use this when automatic segmentation produces incorrect splits —
for example at transitions between surface types that are ambiguous from
curvature alone.

### Step-by-step

1. Go to **Reverse Engineering → Segmentation → Manual Segmentation…**.
2. The task panel shows a list of segments and brush controls.
3. Select a mesh from the **Shapes** list.
4. Choose or create a **Segment** from the Segments list.
5. Use the brush (left-click drag in the 3-D view) to paint faces onto the
   selected segment.
6. Repeat for each segment region.
7. Click **OK** to create the segment objects.

### Brush controls

| Control | Action |
|---------|--------|
| Left-click drag | Add faces to the current segment |
| Right-click drag | Remove faces from the current segment |
| Scroll wheel | Increase/decrease brush size |

---

## From Components {#from-components}

**Menu:** Reverse Engineering → Segmentation → From Components  
**Command:** `Reen_SegmentationFromComponents`

Creates one mesh segment per **connected component** of the input mesh. A
connected component is a set of faces all reachable from each other through
shared edges — i.e. a geometrically disconnected sub-mesh.

Use this when:
- The mesh was already broken apart in an earlier step.
- You imported a mesh file that contains multiple separate objects as one
  combined mesh.
- You used the Mesh workbench's **Split by Components** tool and want the
  resulting parts as individual objects for fitting.

### Step-by-step

1. Select one or more `Mesh::Feature` objects.
2. Go to **Reverse Engineering → Segmentation → From Components**.
3. FreeCAD creates a group **Segments \<name\>** containing one
   `Mesh::Feature` per disconnected component.

!!! note
    If the mesh is one connected piece, this creates a group with a single
    member — the original mesh. It is not an error; it just confirms the mesh
    is already connected.

---

## Wire From Mesh Boundary… {#wire-from-mesh-boundary}

**Menu:** Reverse Engineering → Segmentation → Wire From Mesh Boundary…  
**Command:** `Reen_MeshBoundary`

Extracts the **boundary loops** of a mesh as `Part` geometry. A boundary loop
is a chain of mesh edges that have only one adjacent face — i.e. a hole in the
mesh surface.

The result is either:
- A `Part::Feature` with a **face** (if the boundary loops form a planar
  profile that can be filled).
- A `Part::Feature` with a **compound of wires** (if the loops are non-planar
  or cannot be filled).

### Use cases

- Recovering profile curves from a mesh that was modelled as an open surface.
- Generating a closing face to make a mesh watertight before further processing.
- Using the boundary wire as a reference sketch in Part Design.

### Step-by-step

1. Select one or more `Mesh::Feature` objects.
2. Go to **Reverse Engineering → Segmentation → Wire From Mesh Boundary…**.
3. FreeCAD computes the boundary loops and creates a `Part::Feature`:
   - If all loops are co-planar and form a valid face: a face is created.
   - Otherwise: a compound of wires is created.

### Python API

The boundary extraction uses `MeshAlgorithm::GetMeshBorders` from MeshCore.
Access it via the Mesh Python bindings:

```python
import Mesh, Part

mesh = App.ActiveDocument.getObject("MyMesh").Mesh
# Boundary extraction is done internally by the command;
# for scripted access, use the MeshPart module:
import MeshPart
shape = MeshPart.meshToPart(mesh)  # converts the full mesh to a shell
```

---

## Common mistakes and pitfalls

!!! warning "Segmentation produces too many tiny regions"
    **Cause:** The mesh has surface noise that makes smooth areas appear as
    many small curvature patches.  
    **Fix:** Increase the **Min region size** threshold, or smooth the mesh
    with the Mesh workbench **Smooth Mesh** tool before segmenting.

!!! warning "Mesh Segmentation misclassifies a cylinder as free-form"
    **Cause:** The cylinder tolerances are too tight for the curvature noise in
    the mesh, so the cylinder faces are classified as free-form.  
    **Fix:** Increase Cylinder tolerance, or use Manual Segmentation to
    manually paint the cylinder region.

!!! warning "Wire From Mesh Boundary produces a compound of wires instead of a face"
    **Cause:** The boundary loops are non-planar (they lie on a curved surface)
    so FaceMakerCheese cannot fill them with a flat face.  
    **Fix:** The wire compound can still be used as a guide in the Surface
    workbench's Filling tool to create a non-planar face.

---

## See also

- [Surface Reconstruction](surface-reconstruction.md) — reconstruct a mesh first
- [Approximation](approximation.md) — fit surfaces to the resulting segments
