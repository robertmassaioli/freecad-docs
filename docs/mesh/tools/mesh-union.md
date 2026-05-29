# Mesh Union

> **In one sentence:** Compute the Boolean union of two solid meshes,
> producing a single mesh that encloses the combined volume.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Boolean → Union  
**Command:** `Mesh_Union`  
**Shortcut:** none

Computes the Boolean union of two selected `Mesh::Feature` objects. The result
is a new mesh enclosing the volume of both inputs, with the overlapping region
merged into a single continuous surface.

---

## Intuition

Mesh Union is the mesh equivalent of Part Boolean Fuse — it combines two mesh
volumes into one. Unlike the Part operation (which is exact), mesh booleans are
approximate — their quality depends on the triangle density at the intersection
boundary.

Always run [Evaluate and Repair](evaluate-and-repair.md) on both inputs before
attempting Mesh Union.

---

## When to use it

- Combining two solid mesh objects into one unified mesh for 3-D printing.
- Creating complex shapes by uniting simpler mesh primitives.

## When NOT to use it

- **Don't use it on non-solid meshes** — both inputs must be watertight (closed,
  manifold). Run [Evaluate Solid](evaluate-solid.md) first.
- **Don't use it when exact geometry matters** — use Part workbench Boolean
  Fuse for engineering-accurate results.

---

## Step-by-step

1. Select the first mesh in the model tree.
2. **Ctrl+click** the second mesh.
3. Choose **Mesh → Meshes → Boolean → Union**.
4. A new mesh object appears with the union. The original meshes are retained.

---

## Common mistakes and pitfalls

!!! warning "Both meshes must be solid (watertight)"
    If either mesh has holes, non-manifold edges, or self-intersections, the
    Boolean fails silently or produces a corrupt result. Run
    [Evaluate and Repair](evaluate-and-repair.md) on both meshes first.

!!! warning "Non-overlapping solids produce a compound mesh, not a fused solid"
    If the two meshes don't intersect, the union result contains both meshes
    as disconnected components — not a single topologically fused solid.

---

## Python API

```python
import Mesh
import FreeCAD as App

m1 = App.ActiveDocument.getObject("Mesh")
m2 = App.ActiveDocument.getObject("Mesh001")

result = m1.Mesh.copy()
result.unite(m2.Mesh)

feature = App.ActiveDocument.addObject("Mesh::Feature", "Union")
feature.Mesh = result
App.ActiveDocument.recompute()
```

---

## See also

- [Mesh Difference](mesh-difference.md) — subtract one mesh from another
- [Mesh Intersection](mesh-intersection.md) — keep only the shared volume
- [Evaluate and Repair](evaluate-and-repair.md) — verify meshes before boolean operations
- [Mesh Workbench](../index.md) — workbench overview
