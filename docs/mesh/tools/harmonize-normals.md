# Harmonize Normals

> **In one sentence:** Propagate a consistent normal direction across all
> triangles in the mesh, fixing inconsistent orientations from imports or
> Boolean operations.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Harmonize Normals  
**Command:** `Mesh_HarmonizeNormals`  
**Shortcut:** none

Analyses the connectivity of the mesh and propagates a consistent normal
direction across all triangles. The algorithm picks a seed triangle and
flips adjacent triangles whose normals disagree by more than 90°.

---

## Intuition

Triangle normals define which side of each face is the "outside". An STL from
a poorly configured exporter may have some triangles facing inward and others
facing outward — the mesh looks fine in wireframe but renders black in places
and fails Boolean operations. Harmonize Normals propagates a single consistent
winding direction across the whole connected mesh.

If the result is uniformly inside-out after harmonizing, follow up with
[Flip Normals](flip-normals.md).

---

## When to use it

- After importing a mesh with inconsistent normals (mixed winding).
- After mesh Boolean operations that may produce mixed-orientation triangles.
- When [Evaluate and Repair](evaluate-and-repair.md) reports "Orientations"
  problems.

## When NOT to use it

- **Don't use it on open meshes with holes** — Harmonize Normals works best
  on closed, connected meshes. On open meshes, the result may be inconsistent.
  Fill holes first.

---

## Python API

```python
import FreeCAD as App

mesh_obj = App.ActiveDocument.getObject("Mesh")
mesh_obj.Mesh.harmonizeNormals()
mesh_obj.touch()
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Works best on manifold, connected meshes"
    On open meshes (with holes) or meshes with multiple disconnected
    components, the propagation may give inconsistent results. Fix holes and
    merge components first.

!!! warning "Result may be uniformly inside-out"
    Harmonize Normals makes all normals consistent but cannot determine which
    direction is "outward" in all cases. If the result looks inside-out,
    apply [Flip Normals](flip-normals.md) to reverse all of them.

---

## See also

- [Flip Normals](flip-normals.md) — reverse all normals after harmonizing
- [Evaluate and Repair](evaluate-and-repair.md) — diagnose normal orientation issues
- [Mesh Workbench](../index.md) — workbench overview
