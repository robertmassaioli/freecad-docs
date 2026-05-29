# Segmentation

> **In one sentence:** Group adjacent triangles with similar normal
> directions and curvature into labelled regions — flat, cylindrical,
> spherical, or freeform — as a first step for reverse engineering.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Segmentation…  
**Command:** `Mesh_Segmentation`  
**Shortcut:** none

Segments the mesh by grouping adjacent triangles with similar surface normal
directions and curvature properties into named regions. Each region is coloured
distinctly in the 3-D view.

---

## Intuition

A scanned part is one continuous triangle soup. Segmentation identifies which
parts of that soup correspond to recognisable geometric primitives — flat walls,
cylindrical bores, spherical domes — so each region can be fitted independently
with [Segmentation — Best Fit](segmentation-best-fit.md) or the Reverse
Engineering workbench.

---

## When to use it

- First step in a reverse-engineering workflow: identify surface types before
  fitting exact primitives.
- Isolating a specific surface region (e.g., a bore) for curvature measurement.
- Analysing the surface composition of an imported scan.

## When NOT to use it

- **Don't use it on noisy meshes without smoothing first** — scan noise
  creates spurious curvature variations that corrupt the segmentation.
  Apply [Smoothing](smoothing.md) before segmenting.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Segmentation…**.
3. Configure which surface types to detect (plane, cylinder, sphere, freeform)
   and their curvature thresholds.
4. Click **OK**. Regions are coloured in the 3-D view.
5. Follow up with [Segmentation — Best Fit](segmentation-best-fit.md) to fit
   exact primitives to each region.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Smooth mesh | Pre-smooth before segmenting to reduce noise |
| Plane | Detect flat regions; set max plane curvature |
| Cylinder | Detect cylindrical regions; set min/max cylinder radius |
| Sphere | Detect spherical regions; set min/max sphere radius |
| Freeform | Classify remaining triangles as freeform |
| Max curvature | Maximum curvature for freeform classification |

---

## Common mistakes and pitfalls

!!! warning "Smooth the mesh before segmenting"
    Scan noise produces high-frequency curvature variations. Without
    smoothing, many triangles are misclassified as "freeform". Apply
    [Smoothing](smoothing.md) first.

!!! warning "Segmentation is sensitive to threshold settings"
    A very tight curvature threshold for "plane" will miss slightly curved
    flat faces. Start with generous thresholds and tighten as needed.

---

## See also

- [Segmentation — Best Fit](segmentation-best-fit.md) — fit exact primitives to the detected regions
- [Vertex Curvature](vertex-curvature.md) — visualise curvature before segmenting
- [Smoothing](smoothing.md) — reduce noise before segmenting
- [Mesh Workbench](../index.md) — workbench overview
