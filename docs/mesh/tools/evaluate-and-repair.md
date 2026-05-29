# Evaluate and Repair

> **In one sentence:** Open a comprehensive dialog that checks a mesh for
> all common defect types and offers one-click repair for each.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Analyze → Evaluate & Repair…  
**Command:** `Mesh_Evaluation`  
**Shortcut:** none

Opens the Evaluate & Repair dialog — the most comprehensive mesh analysis
tool. It checks for multiple defect types and offers one-click repair for each.

---

## Intuition

Every STL file from an external tool, photogrammetry pipeline, or CT scan
should be evaluated before further use. Common defects — holes, non-manifold
edges, inverted normals — cause printing failures and invalid Boolean results.
Evaluate and Repair finds and fixes them in one place.

Run it immediately after [Import Mesh](import-mesh.md) and after any mesh
Boolean or cutting operation.

---

## When to use it

- After importing any external mesh file.
- Before attempting mesh Boolean operations (Union, Difference, Intersection).
- After mesh cutting or trimming operations.
- Before converting to a solid with Part → [Shape from Mesh](../../part/tools/shape-from-mesh.md).

---

## Step-by-step

1. Select the mesh in the model tree.
2. Choose **Mesh → Meshes → Analyze → Evaluate & Repair…**.
3. Click **Analyze** (runs all checks simultaneously).
4. Review the report — green checkmarks = OK; red = defects found.
5. For each defect, click **Repair** to apply the automatic fix.
6. Re-run **Analyze** to confirm all issues are resolved.
7. Click **Close**.

---

## Checks performed

| Check | What it finds | Repair action |
|-------|--------------|---------------|
| Duplicate points | Vertices at the same/nearby position | Merge nearby points |
| Face indices | Invalid triangle index references | Remove invalid faces |
| Holes | Open boundary edges (unshared edges) | Fill all holes automatically |
| Non-manifold points | Vertices in non-manifold configuration | Remove non-manifold points |
| Non-manifold edges | Edges shared by more than 2 triangles | Remove non-manifold edges |
| Orientations | Inconsistent triangle winding | Harmonize normals |
| Normals | Outward vs inward pointing | Flip normals |
| Self-intersections | Triangles that penetrate each other | — (no automatic fix) |
| Degenerate faces | Zero-area triangles | Remove degenerate faces |

---

## Common mistakes and pitfalls

!!! warning "Self-intersections have no automatic repair"
    Evaluate & Repair detects self-intersections but cannot fix them
    automatically — the geometry is ambiguous. Fix by returning to the
    source CAD tool and re-exporting, or use CloudCompare or MeshLab for
    manual correction.

!!! warning "A mesh can pass all checks and still have self-intersections"
    The solid check, normal check, and hole check can all pass while
    self-intersections remain. Always run the full Evaluate & Repair
    analysis — don't rely on a single check.

!!! warning "Repair order matters"
    Fix duplicate points first — they cause many other defects (non-manifold
    edges, holes) that will disappear once the duplicates are merged.

---

## See also

- [Import Mesh](import-mesh.md) — import meshes to evaluate
- [Evaluate Solid](evaluate-solid.md) — quick solid-closure check
- [Harmonize Normals](harmonize-normals.md) — fix normal inconsistency manually
- [Fill Up Holes](fill-up-holes.md) — fill holes manually
- [Mesh Workbench](../index.md) — workbench overview
