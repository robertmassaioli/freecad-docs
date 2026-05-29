# Mesh Difference

> **In one sentence:** Subtract one solid mesh from another, removing the
> volume of the second mesh from the first.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Boolean → Difference  
**Command:** `Mesh_Difference`  
**Shortcut:** none

Computes the Boolean difference — removes the volume of the second mesh from
the first. Selection order matters: the first selected mesh is the base; the
second is the cutter.

---

## Intuition

Mesh Difference is the mesh equivalent of Part Boolean Cut. It is useful for
cutting holes or slots through a solid mesh shape using another mesh as the
cutter. Unlike the Part operation, it works directly on triangle meshes without
converting to B-rep.

---

## When to use it

- Cutting a slot or hole through a solid mesh.
- Subtracting a tool shape from a workpiece mesh.
- Creating negative-space features in an STL for printing.

## When NOT to use it

- **Don't use it when exact geometry matters** — use Part workbench
  Boolean Cut for engineering-accurate results.
- **Both meshes must be solid** — open meshes produce corrupt results.

---

## Step-by-step

1. Select the **base mesh** (the mesh to cut from).
2. **Ctrl+click** the **tool mesh** (the cutter).
3. Choose **Mesh → Meshes → Boolean → Difference**.
4. A new mesh appears with the tool volume removed.

!!! warning "Selection order matters"
    First selected = base (what gets cut from). Second selected = tool
    (what is subtracted). Reversing the order cuts from the wrong mesh.

---

## Common mistakes and pitfalls

!!! warning "Both meshes must be solid"
    Run [Evaluate Solid](evaluate-solid.md) on both meshes first.
    Open meshes produce silent failures or corrupt results.

!!! warning "The tool mesh must intersect the base"
    If the tool mesh is entirely outside the base, the result is identical
    to the base (no material is removed).

---

## Python API

```python
import Mesh
import FreeCAD as App

base = App.ActiveDocument.getObject("Mesh")
tool = App.ActiveDocument.getObject("Mesh001")

result = base.Mesh.copy()
result.subtract(tool.Mesh)

feature = App.ActiveDocument.addObject("Mesh::Feature", "Difference")
feature.Mesh = result
App.ActiveDocument.recompute()
```

---

## See also

- [Mesh Union](mesh-union.md) — combine both mesh volumes
- [Mesh Intersection](mesh-intersection.md) — keep only the shared volume
- [Evaluate and Repair](evaluate-and-repair.md) — verify meshes before boolean operations
- [Mesh Workbench](../index.md) — workbench overview
