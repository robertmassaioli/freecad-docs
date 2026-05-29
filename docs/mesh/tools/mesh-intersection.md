# Mesh Intersection

> **In one sentence:** Compute the Boolean intersection of two solid meshes,
> keeping only the volume where both meshes overlap.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Boolean → Intersection  
**Command:** `Mesh_Intersection`  
**Shortcut:** none

Computes the Boolean intersection of two selected meshes — the mesh that
represents only the volume where both inputs overlap. If the meshes do not
overlap, the result is empty.

---

## Intuition

Mesh Intersection is the mesh equivalent of Part Boolean Common — it keeps
only the material shared by both inputs. Use it to find the common volume
between two overlapping scan meshes or to compute the portion of a mesh
that fits within a bounding region.

---

## When to use it

- Finding the common volume between two overlapping scan meshes.
- Clipping a mesh to a bounding region.

## When NOT to use it

- **Don't use it on non-overlapping meshes** — the result is empty.
- **Don't use it on non-solid meshes** — both inputs must be watertight.

---

## Step-by-step

1. Select the first mesh.
2. **Ctrl+click** the second mesh.
3. Choose **Mesh → Meshes → Boolean → Intersection**.
4. A new mesh appears containing only the shared volume.

---

## Common mistakes and pitfalls

!!! warning "Both meshes must be solid"
    Run [Evaluate Solid](evaluate-solid.md) on both meshes before
    attempting intersection.

!!! warning "Non-overlapping meshes produce an empty result"
    Verify that the meshes actually share a volume before running the
    intersection.

---

## See also

- [Mesh Union](mesh-union.md) — combine both mesh volumes
- [Mesh Difference](mesh-difference.md) — subtract one mesh from another
- [Evaluate and Repair](evaluate-and-repair.md) — verify meshes first
- [Mesh Workbench](../index.md) — workbench overview
