# Convert FEM Mesh to Mesh

> **In one sentence:** Export the surface of the FEM mesh as a standard
> FreeCAD mesh object for STL export or visual inspection.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh → Convert FEM Mesh to Mesh

Converts the surface of the FEM mesh into a standard FreeCAD `Mesh::Feature`
object. Useful for exporting the mesh surface, visualising it in other
workbenches, or checking mesh coverage before solving.

---

## Use cases

- Export the mesh surface to STL or OBJ for external use.
- Visualise the mesh in the Mesh workbench for quality inspection.
- Quick visual check of mesh coverage and element size before running the solver.

---

## See also

- [Mesh from Shape (Netgen)](mesh-netgen.md) — generate the FEM mesh
- [Mesh from Shape (Gmsh)](mesh-gmsh.md) — generate the FEM mesh
- [FEM Workbench](../index.md) — workbench overview
