# Remove Component by Hand

> **In one sentence:** Click on individual mesh components to select and
> delete them one by one.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Remove Component by hand  
**Command:** `Mesh_RemoveCompByHand`  
**Shortcut:** none

Activates a pick mode where you click on mesh regions to highlight and delete
their connected component individually. Better than [Remove Components](remove-components.md)
when you need to keep some small components and delete others of similar size.

---

## Intuition

When the size threshold of Remove Components cannot distinguish between noise
components you want to delete and small features you want to keep (e.g., a
small bracket that happens to be the same triangle count as a noise fragment),
Remove Component by Hand gives you direct visual control — click what you want
gone.

---

## When to use it

- You want to delete specific components that are similar in size to ones
  you want to keep.
- You need visual confirmation before deleting each component.
- Selective cleanup after [Split Components](split-components.md) separated
  the mesh.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Remove Component by hand**.
3. Click on a mesh region — its entire connected component is highlighted.
4. Press **Delete** to remove the highlighted component.
5. Repeat for other components.
6. Press **Escape** to exit the mode.

---

## Common mistakes and pitfalls

!!! warning "Each click selects the entire connected component"
    Clicking a triangle selects all triangles connected to it (the whole
    component). This may be much larger than the triangle you clicked.

!!! warning "Deletion is immediate — use Ctrl+Z to undo"
    Each **Delete** removes the component immediately. Undo with **Ctrl+Z**
    if needed.

---

## See also

- [Remove Components](remove-components.md) — threshold-based batch deletion
- [Split Components](split-components.md) — split mesh into separate objects for model tree management
- [Mesh Workbench](../index.md) — workbench overview
