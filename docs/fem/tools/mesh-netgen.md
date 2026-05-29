# Mesh from Shape (Netgen)

> **In one sentence:** Generate a second-order tetrahedral mesh for a solid
> body using the built-in Netgen mesher — the quickest path to an FEM mesh.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh → Mesh from Shape (Netgen)

Creates a second-order tetrahedral mesh using the Netgen mesher, which is
built into FreeCAD. No external installation is required.

---

## Fineness presets

| Fineness | Use |
|----------|-----|
| Very Coarse | Quick topology check |
| Coarse | Fast preliminary run |
| Moderate | Most structural analyses |
| Fine | Stress concentration areas |
| Very Fine | Convergence verification |

---

## Step-by-step

1. Select the body to mesh (or leave unselected to mesh all bodies).
2. Choose **FEM → Mesh → Mesh from Shape (Netgen)**.
3. Set the **Fineness** and optionally **Max Size** / **Min Size** (mm).
4. Click **OK**.

---

## Second-order elements

Netgen creates **second-order** (10-node) tetrahedral elements by default.
These are more accurate than first-order (4-node) elements at the same mesh
density. Keep second-order elements unless mesh size is severely constrained.

---

## Netgen vs Gmsh

| Feature | Netgen | Gmsh |
|---------|--------|------|
| Installation | Built-in | External |
| Control | Basic | Advanced |
| Boundary layers | No | Yes |
| Preferred for | Quick analysis | Production meshes, CFD |

---

## Python API

```python
doc = App.ActiveDocument
mesh_obj = doc.addObject("Fem::FemMeshShapeNetgenObject", "FEMMesh")
mesh_obj.Shape = doc.getObject("Body")
mesh_obj.MaxSize = 10.0  # mm
mesh_obj.Fineness = 3    # 0=Very Coarse, 5=Very Fine
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Mesh does not update automatically"
    After changing model geometry, right-click the mesh and choose
    **Clear FEM mesh**, then re-run the mesher.

---

## See also

- [Mesh from Shape (Gmsh)](mesh-gmsh.md) — more powerful external mesher
- [FEM Mesh Region](mesh-region.md) — local mesh refinement
- [FEM Workbench](../index.md) — workbench overview
