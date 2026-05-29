# Merge

> **In one sentence:** Combine two or more separate mesh objects into a
> single `Mesh::Feature` containing all their triangles.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Merge  
**Command:** `Mesh_Merge`  
**Shortcut:** none

Combines two or more `Mesh::Feature` objects into a single mesh object. The
meshes are appended into one data structure — they need not be touching or
overlapping. The original mesh objects are retained.

---

## Intuition

Merge is the mesh equivalent of "make a compound" in the Part workbench: it
groups multiple separate mesh objects into one tree entry without altering
their geometry or computing any intersection.

| | Merge | [Mesh Union](mesh-union.md) |
|--|-------|------|
| Combines objects | Yes | Yes |
| Computes topology | No — appends triangles | Yes — resolves intersections |
| Requires solid meshes | No | Yes |
| Result | One object (may be multi-component) | Single fused solid |
| Speed | Instant | Slower |

Use Merge for organisation (one object instead of many). Use Union when you
need a single topologically fused solid mesh.

---

## When to use it

- Combining scan fragments (separate files) into one mesh for analysis.
- Grouping mesh components back together after separate processing.
- Reducing the number of objects in the model tree.

## When NOT to use it

- **Don't use it when you need a fused solid** — use
  [Mesh Union](mesh-union.md) instead.

---

## Step-by-step

1. Select all meshes to merge (hold **Ctrl** to multi-select).
2. Choose **Mesh → Meshes → Merge**.
3. A new `Mesh::Feature` appears containing all triangles. The originals are retained.

---

## Common mistakes and pitfalls

!!! warning "Merge does not fuse meshes"
    The merged object may contain multiple disconnected components. It is
    not a manifold solid. Use [Mesh Union](mesh-union.md) if you need a
    fused solid.

---

## Python API

```python
import Mesh
import FreeCAD as App

m1 = App.ActiveDocument.getObject("Mesh")
m2 = App.ActiveDocument.getObject("Mesh001")

combined = m1.Mesh.copy()
combined.merge(m2.Mesh)

merged = App.ActiveDocument.addObject("Mesh::Feature", "Merged")
merged.Mesh = combined
App.ActiveDocument.recompute()
```

---

## See also

- [Split Components](split-components.md) — reverse of Merge: split into separate objects
- [Mesh Union](mesh-union.md) — fuse meshes with topology resolution
- [Mesh Workbench](../index.md) — workbench overview
