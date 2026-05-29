# Evaluate Solid

> **In one sentence:** Check whether a mesh is a closed, manifold solid —
> a requirement for watertight 3-D printing and valid Boolean operations.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Analyze → Check Solid  
**Command:** `Mesh_EvaluateSolid`  
**Shortcut:** none

Checks whether the selected mesh is a closed, manifold solid. Reports "Solid"
if all edges are shared by exactly two triangles, or "Not solid" if any open
boundaries exist.

---

## Intuition

For a mesh to be 3-D printable and for Boolean operations to work correctly,
it must be "watertight" — no holes, every edge shared by exactly two triangles.
Evaluate Solid is a quick yes/no check for this property.

Use it as a fast sanity check before attempting mesh Booleans or before
exporting for printing. For a full defect analysis, use
[Evaluate and Repair](evaluate-and-repair.md).

---

## When to use it

- Quick check before attempting mesh Boolean operations (Union, Difference).
- Confirming a repaired mesh is now watertight.
- Verifying that a tessellated solid ([From Part Shape](from-part-shape.md))
  produced a closed mesh.

---

## Step-by-step

1. Select the mesh in the model tree.
2. Choose **Mesh → Meshes → Analyze → Check Solid**.
3. A dialog reports either **Solid** (closed, manifold) or **Not solid**
   (open boundaries exist).

---

## Common mistakes and pitfalls

!!! warning "Solid check does not detect self-intersections"
    A mesh can pass the solid check (closed, manifold) while still having
    self-intersections that will cause slicer errors. Always run the full
    [Evaluate and Repair](evaluate-and-repair.md) for a complete check.

!!! warning "'Not solid' means there are holes"
    Use [Evaluate and Repair](evaluate-and-repair.md) or
    [Fill Up Holes](fill-up-holes.md) to locate and fix the open boundaries.

---

## See also

- [Evaluate and Repair](evaluate-and-repair.md) — full mesh quality check
- [Fill Up Holes](fill-up-holes.md) — fix holes that cause "Not solid"
- [Mesh Union](mesh-union.md) — requires solid meshes as input
- [Mesh Workbench](../index.md) — workbench overview
