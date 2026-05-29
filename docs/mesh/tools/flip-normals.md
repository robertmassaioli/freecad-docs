# Flip Normals

> **In one sentence:** Reverse the orientation of all triangles in the mesh,
> fixing a uniformly inside-out solid.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Flip Normals  
**Command:** `Mesh_FlipNormals`  
**Shortcut:** none

Reverses the winding direction of every triangle in the mesh. All normals that
were pointing inward now point outward, and vice versa.

---

## Intuition

If a mesh looks correct in wireframe but renders black (normals all pointing
away from the camera) or behaves unexpectedly in Boolean operations (subtracting
adds material, or vice versa), all normals are pointing the wrong way. Flip
Normals corrects this in one step.

Flip Normals reverses **all** normals uniformly. If only some normals are wrong
(inconsistent), use [Harmonize Normals](harmonize-normals.md) first to make
them consistent, then Flip Normals if they ended up all pointing inward.

---

## When to use it

- The mesh renders black or very dark (normals pointing away from the viewer).
- After [Harmonize Normals](harmonize-normals.md), the result is uniformly
  inside-out.
- A Boolean operation produces the inverse of the expected result.

## When NOT to use it

- **Don't use it when only some normals are wrong** — use
  [Harmonize Normals](harmonize-normals.md) to make them consistent first.

---

## Python API

```python
import FreeCAD as App

mesh_obj = App.ActiveDocument.getObject("Mesh")
mesh_obj.Mesh.flipNormals()
mesh_obj.touch()
App.ActiveDocument.recompute()
```

---

## See also

- [Harmonize Normals](harmonize-normals.md) — fix inconsistent (mixed) normals
- [Evaluate and Repair](evaluate-and-repair.md) — full mesh diagnosis
- [Mesh Workbench](../index.md) — workbench overview
