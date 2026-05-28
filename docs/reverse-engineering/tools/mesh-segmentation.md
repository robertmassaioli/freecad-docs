# Mesh Segmentation

> **In one sentence:** Automatically split a mesh into regions by classifying
> each face as plane, cylinder, sphere, or free-form using curvature analysis.

---

## Overview

**Workbench:** Reverse Engineering  
**Menu:** Reverse Engineering → Segmentation → Mesh Segmentation…  
**Shortcut:** none

Runs a region-growing algorithm that classifies mesh faces by their principal
curvatures, groups adjacent faces of the same type into segments, and creates
one `Mesh::Feature` per segment inside a group named "Segments". The segments
can then be fed individually to the Approximation tools.

---

## Intuition

Curvature reveals what type of surface a face belongs to:

- A face with near-zero curvature in both directions → flat plane
- A face with curvature in one direction and zero in the other → cylinder or cone
- A face with equal curvature in both directions → sphere

Mesh Segmentation measures these curvature signatures, assigns a label to each
face, then groups connected faces with the same label into a segment. Think
of it as automatically colour-coding your mesh by surface type.

---

## When to use it

- Your scan contains recognisable geometric primitives (flat faces, cylindrical
  bores, spherical fillets) and you want to fit analytic surfaces to them.
- You want to divide a complex scan into manageable regions before applying
  B-spline fitting to each region.
- You are automating a scan-to-CAD pipeline and need region labels for each
  primitive type.

---

## When NOT to use it

- **Highly noisy meshes** — noise inflates the apparent curvature of flat
  faces. Smooth the mesh first (Mesh workbench → Smoothing).
- **When you need manual control** — use [Manual Segmentation](manual-segmentation.md)
  to paint regions interactively.
- **When the mesh is already split into components** — use
  [From Components](from-components.md) instead.

---

## Step-by-step

1. Select a single `Mesh::Feature`.
2. Choose **Reverse Engineering → Segmentation → Mesh Segmentation…**.
3. In the task panel, set:
   - **Smoothing steps** — number of Laplacian smoothing passes before
     curvature estimation (start with 3–5).
   - **Plane / Cylinder / Sphere tolerance** — curvature thresholds for each
     type (start with defaults and adjust if segments are wrong).
   - **Min region size** — discard segments with fewer faces than this.
4. Click **OK**. A "Segments" group is created with one `Mesh::Feature` per region.

!!! tip
    Run Mesh workbench → Smoothing (1–3 iterations) on the input mesh before
    segmenting. Even a little smoothing dramatically improves classification
    accuracy on real-world scan data.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Smoothing steps | Integer | 3 | Laplacian smoothing passes before curvature estimation |
| Plane tolerance | Float | 0.01 | Max curvature magnitude for planar classification |
| Cylinder tolerance | Float | 0.01 | Curvature asymmetry tolerance for cylinder classification |
| Sphere tolerance | Float | 0.01 | Deviation from equal-curvature for sphere classification |
| Min region size | Integer | 100 | Segments with fewer faces are discarded |

---

## Common mistakes and pitfalls

!!! warning "Too many tiny segments"
    **Cause:** Surface noise causes smooth faces to be split into many small
    patches with varying curvature labels.  
    **Fix:** Increase smoothing steps and Min region size. Apply Mesh
    workbench smoothing before segmenting.

!!! warning "Cylinders classified as free-form"
    **Cause:** Cylinder tolerance is too tight for the curvature noise level
    in the mesh.  
    **Fix:** Increase Cylinder tolerance, or use Manual Segmentation to paint
    cylinder regions.

!!! warning "Result has many unlabelled 'free-form' segments"
    **Cause:** The mesh has blending fillets, transitions, or complex surfaces
    that don't match any primitive type.  
    **Fix:** This is expected for complex geometry. Apply
    [Approximate B-Spline Surface](approx-bspline-surface.md) to the
    free-form segments.

---

## Python API

```python
# Mesh Segmentation is command-driven; trigger via GUI command:
import FreeCADGui as Gui
Gui.doCommand("import ReverseEngineering")
Gui.doCommand("Gui.runCommand('Reen_Segmentation', 0)")
```

---

## See also

- [Manual Segmentation](manual-segmentation.md) — paint regions interactively
- [From Components](from-components.md) — one segment per disconnected component
- [Approximate Plane](approx-plane.md) — fit a plane to a planar segment
- [Approximate B-Spline Surface](approx-bspline-surface.md) — fit NURBS to free-form segments
- [Reverse Engineering Workbench](../index.md) — workbench overview
