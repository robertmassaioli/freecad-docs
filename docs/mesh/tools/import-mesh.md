# Import Mesh

> **In one sentence:** Load a mesh file from disk and add it as a
> `Mesh::Feature` object in the active document.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Import…  
**Command:** `Mesh_Import`  
**Shortcut:** none

Reads a mesh file and creates a `Mesh::Feature` in the current document,
ready for visualisation, repair, or export.

---

## Intuition

Mesh Import is the starting point for any mesh workflow: you bring an STL from
a slicer, a PLY from a 3-D scanner, or an OBJ from a graphics tool into FreeCAD
so it can be inspected and repaired before printing or converting to B-rep
geometry.

---

## When to use it

- Loading an STL file for 3-D printing preparation.
- Importing a scan mesh (PLY, OBJ) for quality inspection or reverse engineering.
- Bringing in a mesh from another tool to combine with FreeCAD geometry.

## When NOT to use it

- **Don't use it for point cloud files** — use the Points workbench → Import Points.
- **Don't use it for STEP/IGES/FCStd** — use File → Import or File → Open.

---

## Step-by-step

1. Switch to the **Mesh workbench**.
2. Choose **Mesh → Meshes → Import…**.
3. Navigate to your mesh file and click **Open**.
4. A `Mesh::Feature` appears in the model tree.
5. Run **Evaluate and Repair** immediately to check for defects.

---

## Supported file formats

| Format | Extension | Notes |
|--------|-----------|-------|
| STL | `.stl`, `.ast` | 3-D printing standard; binary or ASCII |
| OBJ | `.obj` | Wavefront; common in 3-D graphics |
| OFF | `.off` | Object File Format; simple triangle list |
| PLY | `.ply` | Polygon File Format; scan data |
| SMESH | `.smesh` | SMESH mesh format |
| NASTRAN | `.nas`, `.bdf` | NASTRAN bulk data mesh |
| Abaqus | `.inp` | Abaqus mesh input file |

---

## Common mistakes and pitfalls

!!! warning "Mesh units are always millimetres"
    If your mesh was exported from a tool that uses inches, it will appear
    25.4× too small. Scale the mesh by 25.4 after import with
    [Scale](scale.md).

!!! warning "Check for defects immediately after import"
    Almost every mesh from an external tool has some defect. Run
    [Evaluate and Repair](evaluate-and-repair.md) before any further
    processing.

---

## Python API

```python
import Mesh
import FreeCAD as App

Mesh.insert("/path/to/model.stl", App.ActiveDocument.Name)
App.ActiveDocument.recompute()
```

---

## See also

- [Export Mesh](export-mesh.md) — save a mesh back to disk
- [Evaluate and Repair](evaluate-and-repair.md) — check imported mesh for defects
- [From Part Shape](from-part-shape.md) — create a mesh from a B-rep solid
- [Mesh Workbench](../index.md) — workbench overview
