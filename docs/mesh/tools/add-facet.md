# Add Facet

> **In one sentence:** Manually add a single triangle to the mesh by
> clicking three vertices in the 3-D view.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Add Triangle  
**Command:** `Mesh_AddFacet`  
**Shortcut:** none

Allows you to manually add a single triangle to the mesh. You click three
vertices in the 3-D view to define the triangle corners, and the triangle
is appended to the mesh.

---

## Intuition

Automated repair tools ([Fill Up Holes](fill-up-holes.md),
[Evaluate and Repair](evaluate-and-repair.md)) handle most mesh defects.
For complex gaps or curved patches that automated tools cannot fill correctly,
Add Facet provides fine-grained manual control — one triangle at a time.

---

## When to use it

- Patching a small hole that the automated fill tool created incorrectly.
- Adding the final triangle to close a complex gap.
- Manual correction of a specific mesh region after other repair tools.

## When NOT to use it

- **Don't use it for large holes** — use [Fill Up Holes](fill-up-holes.md)
  or [Fill Hole Interactively](fill-hole-interactively.md) instead.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Add Triangle**.
3. Click the **first vertex** of the new triangle in the 3-D view.
4. Click the **second vertex**.
5. Click the **third vertex**.
6. The triangle is appended to the mesh.
7. Repeat for additional triangles, or press **Escape** to exit.

!!! warning "Verify after manual editing"
    The new triangle is added without checking for manifold integrity.
    Run [Evaluate and Repair](evaluate-and-repair.md) after manual triangle
    editing to confirm the mesh is still valid.

---

## See also

- [Fill Hole Interactively](fill-hole-interactively.md) — fill whole holes by clicking their boundary
- [Fill Up Holes](fill-up-holes.md) — fill all holes automatically
- [Evaluate and Repair](evaluate-and-repair.md) — verify the mesh after editing
- [Mesh Workbench](../index.md) — workbench overview
