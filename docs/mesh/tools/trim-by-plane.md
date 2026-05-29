# Trim by Plane

> **In one sentence:** Trim the mesh at a plane defined by three picked
> points, discarding one half and keeping the other.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Cutting → Trim by Plane  
**Command:** `Mesh_TrimByPlane`  
**Shortcut:** none

Trims the mesh by a plane you define interactively by picking three points in
the 3-D view. One side of the plane is kept; the other is discarded.

---

## Intuition

Trim by Plane is like using a bandsaw with a flat blade aligned to any arbitrary
plane: you define the plane by picking three points, choose which half to keep,
and the operation cuts the mesh cleanly at that plane.

Unlike [Cut by Polygon](cut-by-polygon.md) (a 2-D screen-space operation),
Trim by Plane works in 3-D space and can cut at any angle.

---

## When to use it

- Removing the bottom of a scan mesh to create a flat base.
- Cutting a mesh at a parting plane for mould analysis.
- Trimming excess scan data above or below a reference plane.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Cutting → Trim by Plane**.
3. Pick **three points** in the 3-D view to define the cutting plane.
4. FreeCAD previews the plane and asks which side to keep.
5. Click **Above** or **Below** to discard one side.

---

## Common mistakes and pitfalls

!!! warning "Result has an open boundary"
    Trimming leaves an open mesh edge at the cut plane. Use
    [Fill Up Holes](fill-up-holes.md) to close it.

!!! warning "Three-point plane definition can be imprecise"
    Picking points on a curved or noisy mesh surface may produce a
    slightly tilted plane. For a precise axis-aligned cut, place datum
    points at known coordinates first.

---

## See also

- [Section by Plane](section-by-plane.md) — create a cross-section wire without trimming
- [Cut by Polygon](cut-by-polygon.md) — screen-space polygon cut
- [Fill Up Holes](fill-up-holes.md) — close the open boundary after trimming
- [Mesh Workbench](../index.md) — workbench overview
