# Mesh

> **In one sentence:** The mesh divides the 3-D geometry into small finite
> elements that the solver processes — mesh quality determines both the
> accuracy of results and the time required to solve.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh

| Tool | Description |
|------|-------------|
| [Mesh from Shape (Netgen)](#mesh-from-shape-netgen) | Generate a tetrahedral mesh using the built-in Netgen mesher |
| [Mesh from Shape (Gmsh)](#mesh-from-shape-gmsh) | Generate a mesh using the Gmsh mesher |
| [FEM Mesh Boundary Layer](#fem-mesh-boundary-layer) | Add a boundary layer refinement (Gmsh only) |
| [FEM Mesh Region](#fem-mesh-region) | Add a local mesh density refinement |
| [FEM Mesh Group](#fem-mesh-group) | Define a named group of mesh elements |
| [Create Elements Set](#create-elements-set) | Define a set for selective output or constraints |
| [Convert FEM Mesh to Mesh](#convert-fem-mesh-to-mesh) | Export the FEM mesh as a surface mesh object |

---

## Intuition

### What is a finite element mesh?

The solver cannot operate on continuous geometry — it operates on a discrete
grid. The mesh converts the smooth boundary representation (B-rep) of a
Part Design body into thousands or millions of small elements (usually
tetrahedra for 3-D solids). Each element has nodes at its corners and edges;
the solver computes the unknown field (displacement, temperature, potential)
at each node, then interpolates within each element.

### Mesh quality

**Good mesh → accurate results. Bad mesh → wrong results.**

Key mesh quality indicators:

| Indicator | Good range | Poor value effect |
|-----------|-----------|------------------|
| Element aspect ratio | ≤ 5 | Distorted elements → poor approximation |
| Element size vs feature size | ≥ 3–4 elements across feature | Too few elements miss stress gradient |
| Element order | Second-order (quadratic) | First-order underestimates stress |
| Mesh density at stress concentrations | Fine (2–5× base size) | Misses peak stress |

### Netgen vs Gmsh

| Feature | Netgen | Gmsh |
|---------|--------|------|
| Installation | Built-in to FreeCAD | External install required |
| Meshing control | Basic (global size + refinement) | Advanced (field-based, structured) |
| Boundary layers | Not supported | Supported |
| Mesh types | Tetrahedral only | Tet, hex, prism, structured |
| Speed | Fast for simple parts | Faster for complex parts with control |
| Preferred when | Quick first analysis | Production meshes, fluid flow |

---

## Mesh from Shape (Netgen)

**Menu:** FEM → Mesh → Mesh from Shape (Netgen)

Creates a second-order tetrahedral mesh using Netgen — the quickest way to
get a mesh for a solid structural analysis.

### Step-by-step

1. Select the body or shape to mesh in the model tree (or leave unselected
   to mesh all bodies).
2. Choose **FEM → Mesh → Mesh from Shape (Netgen)**.
3. The Netgen mesh task panel opens.
4. Set the **Fineness** (Very Coarse → Very Fine, or Custom) and optionally
   the **Max Size** and **Min Size** (in mm).
5. Click **OK**. Netgen generates the mesh and it appears in the model tree.

### Fineness presets

| Fineness | Approximate element size | Use |
|----------|-------------------------|-----|
| Very Coarse | ~20% of bounding box | Quick topology check |
| Coarse | ~10% | Fast preliminary run |
| Moderate | ~5% | Most structural analyses |
| Fine | ~2% | Stress concentration areas |
| Very Fine | ~1% | Convergence verification |

### Second-order elements

Netgen creates **second-order** (10-node) tetrahedral elements by default.
Second-order elements have mid-edge nodes that allow the displacement field
to vary quadratically within each element — significantly more accurate than
first-order (4-node) elements for the same mesh density. Keep second-order
elements enabled unless mesh size is severely constrained.

### Python API

```python
import FreeCAD as App
import femmesh.netgen_mesh_shaper as netgen_mesh

doc = App.ActiveDocument
body = doc.getObject("Body")
mesh_obj = doc.addObject("Fem::FemMeshShapeNetgenObject", "FEMMesh")
mesh_obj.Shape = body
mesh_obj.MaxSize = 10.0   # mm maximum element edge length
mesh_obj.Fineness = 3     # 0=Very Coarse, 5=Very Fine
doc.recompute()
```

---

## Mesh from Shape (Gmsh)

**Menu:** FEM → Mesh → Mesh from Shape (Gmsh)

Creates a mesh using the external Gmsh mesher. Gmsh must be installed and
its executable path set in **Edit → Preferences → FEM → Gmsh**.

### Why use Gmsh over Netgen?

- **Boundary layers** for fluid flow analysis (viscous sub-layer resolution).
- **Mesh field control** — use mathematical fields to grade mesh density
  precisely across the domain.
- **Physical groups** — define named regions in the mesh for selective solver
  output.
- **Structured hex meshes** — for certain geometries, hexahedral elements
  give better results than tetrahedra.

### Step-by-step

1. Select the body or shape.
2. Choose **FEM → Mesh → Mesh from Shape (Gmsh)**.
3. In the task panel:
   - Set **Max Size** and **Min Size** (mm).
   - Set **Order**: 1 (linear) or 2 (quadratic — recommended).
   - Choose **Mesh Algorithm**: Automatic, Delaunay, Frontal, etc.
4. Click **OK** (or **Apply** to preview).

### Mesh algorithms

| Algorithm | Description | Best for |
|-----------|-------------|---------|
| Automatic | Gmsh chooses | General use |
| MeshAdapt | Adapts iteratively | Complex surfaces |
| Delaunay | Classical Delaunay tet | Smooth geometries |
| Frontal | Advancing front | Thin features |
| HXT | Parallel Delaunay | Large meshes (fast) |
| Packing of Parallelepipeds | Hex-dominant | Structured-like meshes |

---

## FEM Mesh Boundary Layer

**Menu:** FEM → Mesh → FEM Mesh Boundary Layer

Adds a prismatic boundary layer stack to selected faces in a Gmsh mesh.
Boundary layers are thin, structured layers of elements adjacent to walls —
essential for fluid flow (CFD) analysis to resolve the velocity gradient in
the viscous sublayer.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Start Size | Thickness of the first layer (closest to the wall) |
| Growth Rate | Ratio between successive layer thicknesses (e.g. 1.2) |
| Number of Layers | Total number of prismatic layers |
| References | Faces to apply the boundary layer to |

### Choosing boundary layer parameters

For turbulent flow at moderate Reynolds numbers:

- The first cell height should place the first node at y⁺ ≈ 1 (viscous
  sublayer) or y⁺ ≈ 30–100 (log-law region, for wall functions).
- A growth rate of 1.2–1.3 is typical.
- 10–20 layers is common.

y⁺ = 1 first-cell height estimate: h₁ = Re^(−7/8) × L × C, where L is
the body length and C is a model-dependent constant.

---

## FEM Mesh Region

**Menu:** FEM → Mesh → FEM Mesh Region

Defines a local mesh refinement region inside a Gmsh or Netgen mesh.
Nodes within the region use a smaller element size than the global mesh.

### When to use it

- Stress concentrations: holes, fillets, notches, sharp re-entrant corners.
- Contact zones.
- Areas where temperature or potential gradients are steep.
- Any region where accuracy matters more than elsewhere.

### Step-by-step

1. Add a **Mesh from Shape (Gmsh)** first.
2. Choose **FEM → Mesh → FEM Mesh Region**.
3. Select the face(s), edge(s), or vertex/vertices to refine around.
4. Enter the local **Max Element Size** (smaller than the global size).
5. Click **OK**. The region appears in the model tree under the mesh.
6. Click **OK** on the Gmsh mesh dialog to regenerate with the refinement.

---

## FEM Mesh Group

**Menu:** FEM → Mesh → FEM Mesh Group

Defines a **named group** of mesh elements (faces, edges, volumes) that is
written into the mesh file. Named groups serve two purposes:

1. **Solver input** — Elmer uses named groups to identify which boundaries
   receive which boundary conditions. Without groups, Elmer cannot associate
   BCs with faces.
2. **Selective output** — request results only for specific groups to reduce
   output file size.

### Step-by-step

1. Add a Gmsh mesh first.
2. Choose **FEM → Mesh → FEM Mesh Group**.
3. Select the face(s) to include in the group.
4. Enter a **Name** for the group (e.g. `inlet`, `wall`, `outlet`).
5. Click **OK**. The group appears under the mesh in the model tree.
6. Regenerate the mesh to embed the groups.

---

## Create Elements Set

**Menu:** FEM → Mesh → Create Elements Set

Defines a named set of individual mesh elements (by element ID) for
post-processing or selective constraint application. Unlike Mesh Group
(which is geometry-based), Elements Set is a manual selection of specific
elements already in the mesh.

Use this for fine-grained control over which elements are included in a
result extraction or selective material zone.

---

## Convert FEM Mesh to Mesh

**Menu:** FEM → Mesh → Convert FEM Mesh to Mesh

Converts the surface of the FEM mesh into a standard FreeCAD mesh object
(`Mesh::Feature`). Useful for:

- Exporting the mesh surface to STL or OBJ for external use.
- Visualising the mesh in the Part workbench or Mesh workbench.
- Quick visual check of mesh coverage before solving.

---

## Common mistakes and pitfalls

!!! warning "Mesh too coarse at stress concentrations"
    A uniform coarse mesh will underestimate peak stresses at holes, fillets,
    and notches — the gradient is too steep for large elements to capture.
    Add a Mesh Region around every stress concentration with an element size
    at least 3–5× smaller than the global size.

!!! warning "Second-order elements are slower but necessary for accuracy"
    First-order (linear) tetrahedral elements severely underestimate
    bending stiffness (they are overly stiff, known as 'shear locking').
    Always use second-order elements for structural analysis unless mesh
    size is the binding constraint.

!!! warning "Gmsh not found"
    If **Mesh from Shape (Gmsh)** produces an error about the Gmsh binary
    not being found, install Gmsh and set the path in
    **Edit → Preferences → FEM → Gmsh → Gmsh binary**.

!!! warning "Mesh does not update automatically"
    After changing model geometry (e.g. modifying a pad length), the mesh
    does NOT update automatically. Right-click the mesh in the model tree
    and choose **Clear FEM mesh**, then re-run the mesher to regenerate.

!!! warning "Boundary layer + tetrahedral mesh compatibility"
    Boundary layers create prismatic elements adjacent to walls. The
    remaining domain is filled with tetrahedra. Most solvers handle this
    hybrid mesh correctly, but verify that the solver and result reader
    support hybrid meshes before relying on boundary layers.

---

## See also

- [Analysis Container](analysis.md) — the analysis must be active before meshing
- [Materials](materials.md) — material properties define the solve; mesh defines the discretisation
- Solver CalculiX — accepts Netgen and Gmsh tetrahedral meshes
- Solver Elmer — requires Gmsh mesh groups to identify boundary condition faces
