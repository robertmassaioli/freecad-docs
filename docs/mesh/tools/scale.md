# Scale

> **In one sentence:** Rescale the selected mesh by a numeric factor to
> correct unit errors or resize the model.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Scale…  
**Command:** `Mesh_Scale`  
**Shortcut:** none

Scales the selected mesh by a uniform numeric factor. The scale is applied
directly to the vertex coordinates — it is not a parametric operation.

---

## Intuition

The most common use case is correcting a unit mismatch: an STL exported in
inches will appear 25.4× too small in FreeCAD (which uses millimetres). Scale
by 25.4 to bring it to the correct size. Verify the result with
[Bounding Box](bounding-box.md).

---

## When to use it

- An imported STL is the wrong size because it was exported in inches.
- You need to uniformly resize a mesh (e.g., scale up by 2×).

## When NOT to use it

- **Don't use it for non-uniform scaling** — this tool applies one factor to
  all three axes. For per-axis scaling, use the Python API or an external tool.
- **Don't use it on parametric geometry** — use the dimensional parameters of
  Part/PartDesign features instead.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Scale…**.
3. Enter the scale factor (e.g. `25.4` for inches to mm).
4. Click **OK**.
5. Verify the new dimensions with [Bounding Box](bounding-box.md).

---

## Common mistakes and pitfalls

!!! warning "Scale is not parametric"
    Scale modifies the mesh in place without a history entry. Use **Ctrl+Z**
    to undo if the wrong factor was applied.

!!! warning "Re-check the bounding box after scaling"
    Always verify the bounding box dimensions after scaling to confirm the
    result matches the expected physical dimensions.

---

## Python API

```python
import FreeCAD as App

mesh_obj = App.ActiveDocument.getObject("Mesh")
msh = mesh_obj.Mesh.copy()
msh.scale(25.4)   # inches to mm
mesh_obj.Mesh = msh
App.ActiveDocument.recompute()
```

---

## See also

- [Bounding Box](bounding-box.md) — verify dimensions before and after scaling
- [Import Mesh](import-mesh.md) — import meshes that may need scaling
- [Mesh Workbench](../index.md) — workbench overview
