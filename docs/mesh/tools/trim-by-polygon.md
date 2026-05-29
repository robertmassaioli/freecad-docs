# Trim by Polygon

> **In one sentence:** Draw a closed polygon on screen and keep only the
> mesh elements inside the polygon boundary, removing everything outside.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Cutting → Trim by Polygon  
**Command:** `Mesh_PolyTrim`  
**Shortcut:** none

Similar to [Cut by Polygon](cut-by-polygon.md), but always removes the mesh
elements outside the drawn polygon and retains the elements inside. The result
has the polygon outline as its boundary.

---

## Intuition

Trim by Polygon is a shorthand for "Cut by Polygon → keep inner". Use it when
you want to crop a mesh down to a specific 2-D outline — for example, keeping
only the scan data within a particular region of interest.

---

## When to use it

- Cropping a scan mesh to a region of interest defined by an outline.
- Removing everything outside a specific screen-drawn boundary in one step.

---

## Step-by-step

1. Align the 3-D view to the cut plane (use a standard view in orthographic
   projection).
2. Choose **Mesh → Meshes → Cutting → Trim by Polygon**.
3. Left-click to add polygon vertices.
4. Right-click to close the polygon — everything outside is removed.

---

## Common mistakes and pitfalls

!!! warning "Cut is performed in 2-D screen space"
    Use orthographic projection and a standard view (Top/Front/Side) for
    predictable results. Perspective or off-axis views produce oblique cuts.

!!! warning "Result has an open boundary"
    The trimmed edge is an open boundary. Use [Fill Up Holes](fill-up-holes.md)
    if you need to close it.

---

## See also

- [Cut by Polygon](cut-by-polygon.md) — same operation with a choice of which side to keep
- [Trim by Plane](trim-by-plane.md) — trim with a 3-D plane
- [Mesh Workbench](../index.md) — workbench overview
