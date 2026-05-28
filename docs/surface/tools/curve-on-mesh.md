# Curve on Mesh

> **In one sentence:** Interactively trace a B-spline curve along the surface
> of a mesh by picking points that snap to the mesh.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface → Curve on Mesh  
**Shortcut:** none

Enters an interactive curve-drawing mode where each click places a control
point that snaps to the mesh surface. The result is a B-spline curve that
follows the mesh topology. Used primarily in reverse-engineering workflows to
extract guide curves from a scan mesh for subsequent surface reconstruction.

---

## Intuition

Think of it like drawing with a pen on a 3-D sculpture: each click plants a
dot on the surface, and FreeCAD connects those dots with a smooth B-spline
that hugs the contours of the mesh. You're not drawing in free space — the
pen always sticks to the surface.

This is the first step in a reverse-engineering workflow: scan → mesh →
extract guide curves → build NURBS surfaces through those curves.

---

## When to use it

- You have an imported scan mesh (STL, OBJ) and want to extract feature
  curves (edges of surfaces, symmetry lines, section profiles) to guide
  surface reconstruction.
- You need boundary curves for [Filling](filling.md) or [Sections](sections.md)
  that follow the actual scan geometry.
- You are tracing a design intent curve on a physical prototype scan.

---

## When NOT to use it

- **On CAD solid geometry** — if you need curves on a B-rep solid, use
  Sketcher (sketch on face) or Draft tools. Curve on Mesh is specifically
  designed for mesh input.
- **For precise feature extraction** — the B-spline approximation has
  tolerance; for exact geometric features, use dedicated scan-to-CAD software.

---

## Step-by-step

1. Import or create a mesh object (use Mesh workbench → Import Mesh or
   Mesh from Shape).
2. Switch to the **Surface workbench**.
3. Select the mesh object.
4. Choose **Surface → Curve on Mesh**.
5. Click points on the mesh surface to trace the desired curve. Each click
   adds a control point that snaps to the mesh.
6. Press **Enter** or right-click to finish and accept the curve.
7. Press **Escape** to cancel without creating a curve.

!!! tip
    Place more control points in regions with high curvature (tight bends,
    sharp features) and fewer points in flat or smoothly curved regions.
    Over-specifying flat regions makes the spline stiff and less fair.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Mesh | Reference | — | The source mesh object that points snap to |
| Segment Size | Float (mm) | — | Approximation tolerance — how closely the B-spline follows the picked points |
| Angle Tolerance | Float (°) | — | Maximum angle between consecutive mesh face normals for a single curve segment |

### Resulting object properties

| Property | Description |
|----------|-------------|
| `Wire` | The B-spline curve draped on the mesh surface |
| `Mesh` | Reference to the source mesh |
| `SegmentSize` | Tolerance used during curve fitting |

---

## Common mistakes and pitfalls

!!! warning "Points don't snap to the mesh"
    **Cause:** The mesh may be offset from the origin, or snap is not active.  
    **Fix:** Ensure the mesh is the selected object before starting, and that
    the mesh is visible. If snapping doesn't work, try rebuilding the mesh
    normals (Mesh workbench → Harmonize Normals).

!!! warning "Curve drifts away from the mesh surface"
    **Cause:** Too few control points in a high-curvature region; the spline
    cuts corners.  
    **Fix:** Add more control points in high-curvature areas to pull the
    B-spline closer to the surface.

!!! warning "Curve on Mesh via Surface menu not the same as Sketcher On Surface"
    **Cause:** Part Design's "Sketcher On Surface" maps a 2D sketch onto a face,
    which is a different operation.  
    **Fix:** Confirm you are in the Surface workbench and using the correct menu
    entry. Curve on Mesh works with mesh objects; Sketcher On Surface works with
    B-rep faces.

---

## Python API

The Curve on Mesh tool is primarily interactive. The resulting object can be
accessed programmatically after creation:

```python
import FreeCAD as App

doc = App.ActiveDocument

# After creating the curve interactively, access the result:
curve = doc.getObject("CurveOnMesh")   # actual name may differ
if curve:
    print("Wire:", curve.Wire)
    print("Segment size:", curve.SegmentSize)

# The wire can be used as a boundary edge for Filling:
fill = doc.addObject("Surface::Filling", "Fill")
fill.BoundaryEdges = [(curve, ["Edge1"])]
doc.recompute()
```

---

## See also

- [Filling](filling.md) — use the extracted curve as a boundary for a surface
- [Sections](sections.md) — loft through multiple extracted curves
- [Surface Workbench](../index.md) — overview of the Surface workbench
- Mesh workbench — import and prepare the scan mesh
