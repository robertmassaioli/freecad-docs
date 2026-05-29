# Remesh with Gmsh

> **In one sentence:** Re-mesh an existing mesh using the external Gmsh
> mesher to improve element quality, uniformity, and size control.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Remesh by Gmsh…  
**Command:** `Mesh_RemeshGmsh`  
**Shortcut:** none

Re-meshes an existing `Mesh::Feature` using the Gmsh mesher. Gmsh produces
higher-quality elements than FreeCAD's built-in tessellator — better triangle
shapes, more uniform sizes, and user-controlled element size bounds.

---

## Intuition

When you import an STL with poor triangle quality (elongated or flat triangles)
or when FreeCAD's built-in tessellator produces uneven element sizes, Gmsh
can re-mesh the surface to produce triangles that are more regular and suitable
for FEM analysis or high-quality rendering.

---

## When to use it

- The imported STL has poor triangle quality (very elongated or flat elements).
- You need uniform element size across the mesh surface for FEA.
- You want to reduce triangle count while maintaining surface accuracy.
- Your FEA solver requires specific element size constraints.

## When NOT to use it

- **Don't use it if Gmsh is not installed** — Gmsh is an external dependency.
  Install it and set its path in preferences first.
- **Don't use it for quick 3-D printing prep** — the built-in tessellator
  ([From Part Shape](from-part-shape.md)) is faster and sufficient for most
  printing workflows.

---

## Step-by-step

1. Ensure Gmsh is installed and its path is set in
   **Edit → Preferences → Mesh → Gmsh**.
2. Select the mesh in the model tree.
3. Choose **Mesh → Meshes → Remesh by Gmsh…**.
4. Set the parameters (see below).
5. Click **OK**. A new `Mesh::Feature` appears with the re-meshed result.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Max element size | Largest triangle edge length (mm) |
| Min element size | Smallest triangle edge length (mm) |
| Angle tolerance | Curvature-driven size reduction at curved regions (°) |

---

## Common mistakes and pitfalls

!!! warning "Gmsh must be installed separately"
    Gmsh is not bundled with FreeCAD. Download it from gmsh.info and set
    its executable path in **Edit → Preferences → Mesh → Gmsh path**
    before using this tool.

!!! warning "Re-meshing changes triangle indices"
    Any references to specific triangle indices from the original mesh
    are invalidated after re-meshing.

---

## See also

- [From Part Shape](from-part-shape.md) — create a mesh from a B-rep solid
- [Evaluate and Repair](evaluate-and-repair.md) — check mesh quality before re-meshing
- [Smoothing](smoothing.md) — alternative mesh quality improvement
- [Mesh Workbench](../index.md) — workbench overview
