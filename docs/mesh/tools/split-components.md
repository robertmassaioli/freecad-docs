# Split Components

> **In one sentence:** Split a mesh that contains multiple disconnected
> components into separate `Mesh::Feature` objects — one per component.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Split by components  
**Command:** `Mesh_SplitComponents`  
**Shortcut:** none

Analyses the selected mesh for disconnected components (separate mesh islands
with no shared edges) and creates one `Mesh::Feature` per component in the
model tree.

---

## Intuition

An imported mesh file may contain many separate objects as one `Mesh::Feature`
— a scan that captured both the part and the scan rig, or a 3-D scene with
multiple objects exported as one STL. Split Components separates them into
independent objects that can be managed, deleted, or processed individually.

---

## When to use it

- An imported mesh file contains multiple separate objects.
- After a Boolean Difference that left disconnected fragments.
- When you need to delete specific components and keep others of similar size
  (use with [Remove Component by Hand](remove-component-by-hand.md)).

## When NOT to use it

- **Don't use it when the mesh has many noise fragments** — hundreds of
  components produce hundreds of objects in the model tree. Use
  [Remove Components](remove-components.md) to eliminate noise first.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Split by components**.
3. One `Mesh::Feature` per connected component appears in the tree.
4. Delete unwanted components manually in the tree.
5. (Optional) [Merge](merge.md) the remaining components back into one.

---

## Workflow: precise component cleanup

```
1. Import mesh → one mesh object with many components
2. Split Components → separate objects per component
3. Delete small noise objects in the model tree
4. Merge → recombine the remaining objects
```

---

## Common mistakes and pitfalls

!!! warning "Remove noise first to avoid flooding the tree"
    An imported scan may have hundreds of tiny noise fragments. Splitting
    creates hundreds of objects. Run [Remove Components](remove-components.md)
    with a size threshold first.

---

## Python API

```python
import Mesh
import FreeCAD as App

doc = App.ActiveDocument
mesh = doc.getObject("Mesh")

components = mesh.Mesh.getSeparateComponents()
for i, comp in enumerate(components):
    feature = doc.addObject("Mesh::Feature", f"Component_{i}")
    feature.Mesh = comp

doc.recompute()
```

---

## See also

- [Merge](merge.md) — combine mesh objects back into one
- [Remove Components](remove-components.md) — batch-delete small components by threshold
- [Remove Component by Hand](remove-component-by-hand.md) — selectively delete components
- [Mesh Workbench](../index.md) — workbench overview
