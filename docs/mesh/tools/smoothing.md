# Smoothing

> **In one sentence:** Apply a Laplacian or Taubin filter to reduce surface
> noise and rough triangulation artifacts in the mesh.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Smooth…  
**Command:** `Mesh_Smoothing`  
**Shortcut:** none

Applies a smoothing filter to the mesh to reduce surface noise and rough
triangulation artifacts. Two algorithms are available: Laplacian (simple,
may shrink the mesh) and Taubin (volume-preserving).

---

## Intuition

A mesh from a 3-D scanner always has some surface noise — high-frequency
variations in vertex positions that don't represent real geometry. Smoothing
moves each vertex toward the average position of its neighbours, reducing the
noise while (ideally) preserving the overall shape.

Use Taubin smoothing for engineering work — it applies a two-step correction
that largely prevents the mesh shrinkage that plain Laplacian smoothing causes.

---

## When to use it

- Reducing noise on a scan mesh before segmentation or reverse-engineering fits.
- Improving the appearance of a mesh for rendering.
- Preparing a mesh for [Segmentation (Curvature)](segmentation-curvature.md)
  which is sensitive to high-frequency noise.

## When NOT to use it

- **Don't over-smooth** — each pass removes surface detail. Sharp edges,
  lettering, and fine features are blurred and permanently lost. Always work
  on a copy.
- **Don't use it expecting to improve mesh topology** — smoothing moves
  vertices but does not fix holes, non-manifold edges, or inverted normals.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Algorithm | Laplacian or Taubin |
| Iterations | Number of smoothing passes (1–20 typical) |
| Lambda | Smoothing factor (0–1); larger = more smoothing per pass |
| Mu | Anti-shrinkage factor (Taubin only); use a negative value |

**Recommended for scan data:**
- Algorithm: Taubin
- Iterations: 5–10
- Lambda: 0.5, Mu: −0.53

---

## Common mistakes and pitfalls

!!! warning "Over-smoothing destroys sharp features"
    Smoothing is irreversible on the current mesh. Sharp edges, embossed
    text, and fine detail are blurred permanently. Always keep a backup copy
    (use [Simple Copy](../../part/tools/simple-copy.md) or duplicate the
    mesh object) before smoothing.

!!! warning "Laplacian smoothing shrinks the mesh"
    Many iterations of Laplacian smoothing gradually shrink the overall
    mesh volume. Use Taubin smoothing (which includes an anti-shrinkage
    correction step) for engineering work.

---

## See also

- [Decimating](decimating.md) — reduce triangle count after smoothing
- [Segmentation (Curvature)](segmentation-curvature.md) — smooth before segmenting
- [Evaluate and Repair](evaluate-and-repair.md) — fix topology before smoothing
- [Mesh Workbench](../index.md) — workbench overview
