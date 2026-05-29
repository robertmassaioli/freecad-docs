# Export Mesh

> **In one sentence:** Save the selected mesh to a file in STL, OBJ, PLY,
> or another supported format.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Export…  
**Command:** `Mesh_Export`  
**Shortcut:** none

Saves the selected `Mesh::Feature` to a file on disk. The output format is
determined by the file extension chosen in the save dialog.

---

## Intuition

Export Mesh is the last step before sending a model to a 3-D printer, slicer,
or external geometry tool. Choose the format that your target application expects
— STL for 3-D printing, OBJ for 3-D graphics tools.

---

## When to use it

- Exporting a tessellated solid to STL for 3-D printing.
- Saving a repaired mesh for use in another application.
- Converting between mesh formats (e.g., PLY → OBJ).

## When NOT to use it

- **Don't use it to export B-rep geometry to STEP or IGES** — use
  File → Export with a B-rep format.

---

## Step-by-step

1. Select the `Mesh::Feature` in the model tree.
2. Choose **Mesh → Meshes → Export…**.
3. In the save dialog, choose the file path and type the filename with
   the desired extension (e.g., `part.stl`).
4. Click **Save**.

---

## Format guide

| Format | Extension | Best for |
|--------|-----------|---------|
| STL (binary) | `.stl` | 3-D printing — compact binary |
| STL (ASCII) | `.ast` | Debugging — human-readable but large |
| OBJ | `.obj` | 3-D graphics tools (Blender, etc.) |
| PLY (binary) | `.ply` | Point cloud / scan data tools |
| OFF | `.off` | Simple geometry exchange |

!!! tip "Prefer binary STL for printing"
    Binary STL is 5–10× smaller than ASCII STL for the same mesh. All
    modern slicers read binary STL.

---

## Common mistakes and pitfalls

!!! warning "Export the repaired mesh, not the raw import"
    If you imported an STL with defects and repaired it, make sure you
    select the repaired mesh object (not the original import) before
    exporting.

---

## Python API

```python
import Mesh
import FreeCAD as App

mesh_obj = App.ActiveDocument.getObject("Mesh")
Mesh.export([mesh_obj], "/path/to/output.stl")
```

---

## See also

- [Import Mesh](import-mesh.md) — load a mesh file
- [From Part Shape](from-part-shape.md) — create a mesh from a solid before export
- [Evaluate and Repair](evaluate-and-repair.md) — verify the mesh before exporting
- [Mesh Workbench](../index.md) — workbench overview
