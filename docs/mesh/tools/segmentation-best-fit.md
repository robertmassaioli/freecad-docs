# Segmentation — Best Fit

> **In one sentence:** Fit the optimal geometric primitive (plane, cylinder,
> or sphere) to each classified mesh segment and report the fit quality.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Segmentation — Best Fit…  
**Command:** `Mesh_SegmentationBestFit`  
**Shortcut:** none

An enhanced version of [Segmentation (Curvature)](segmentation-curvature.md)
that not only classifies mesh regions by surface type but also fits the optimal
geometric primitive to each region and reports the RMS fit error.

---

## Intuition

Where Segmentation (Curvature) just colours regions by type, Best Fit takes the
next step: for each cylindrical region it computes the best-fit axis and radius;
for each planar region it computes the best-fit normal and offset; for each
spherical region it computes the best-fit centre and radius. These parameters
can then be used directly to create exact B-rep primitives in the Part
workbench for reverse engineering.

---

## When to use it

- Reverse engineering: you want to reconstruct an STL as exact B-rep geometry.
- Quality inspection: you want to know how closely a scan matches a nominal
  primitive (cylinder, plane) and by how much it deviates.

## When NOT to use it

- **Don't use it without running Segmentation first** — Best Fit requires
  pre-classified segments from [Segmentation (Curvature)](segmentation-curvature.md).

---

## Step-by-step

1. Run [Segmentation (Curvature)](segmentation-curvature.md) to classify the mesh.
2. Choose **Mesh → Meshes → Segmentation — Best Fit…**.
3. Review the fitted primitives and their RMS fit errors.
4. Use the reported parameters (radius, axis, normal) to create exact Part
   workbench geometry.

---

## Supported primitives

| Primitive | Parameters computed |
|-----------|-------------------|
| Plane | Normal vector, D (distance from origin), RMS fit error |
| Cylinder | Axis direction, Axis point, Radius, RMS fit error |
| Sphere | Centre point, Radius, RMS fit error |

---

## Fit quality interpretation

| RMS fit error | Interpretation |
|---------------|---------------|
| < 0.1 mm | Excellent — the segment closely matches the primitive |
| 0.1–0.5 mm | Acceptable for rough reconstruction |
| > 0.5 mm | Poor — the segment may not be the assumed type, or mesh resolution is too coarse |

---

## Common mistakes and pitfalls

!!! warning "Best Fit requires pre-classified segments"
    Run [Segmentation (Curvature)](segmentation-curvature.md) first to
    identify region types. Best Fit operates on those classified segments.

!!! warning "High RMS error may mean wrong primitive type"
    If a region has RMS > 0.5 mm for a cylindrical fit, the region may be
    a cone, torus, or freeform surface rather than a cylinder. Review the
    segmentation classification.

---

## See also

- [Segmentation (Curvature)](segmentation-curvature.md) — prerequisite: classify regions by surface type
- [Vertex Curvature](vertex-curvature.md) — visualise curvature across the mesh
- [Smoothing](smoothing.md) — reduce noise before segmenting for better fit quality
- [Mesh Workbench](../index.md) — workbench overview
