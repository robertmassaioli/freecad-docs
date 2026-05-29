# Cut by Polygon

> **In one sentence:** Cut the mesh by drawing a closed polygon on the screen,
> removing mesh elements on one side of the projected polygon boundary.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Cutting → Cut by Polygon  
**Command:** `Mesh_PolyCut`  
**Shortcut:** none

Cuts the mesh by drawing a closed polygon in 2-D screen space. The polygon is
projected onto the mesh and used as a cutting boundary. Mesh elements on the
chosen side of the polygon are removed, leaving an open boundary.

---

## Intuition

Think of using a cookie cutter: you stamp a shape from the top and remove
everything outside (or inside) the stamp outline. Cut by Polygon does this
from the current view direction — you draw the outline on screen and FreeCAD
removes the mesh on one side.

The cut results in an open mesh boundary at the cut line. To close it, use
[Fill Up Holes](fill-up-holes.md) or [Fill Hole Interactively](fill-hole-interactively.md).

---

## When to use it

- Removing a region of a scan mesh that is outside your area of interest.
- Cropping a mesh to a specific outline.
- Removing a visible artifact or unwanted protrusion from a scan.

---

## Step-by-step

1. Orient the 3-D view so you're looking straight at the region to cut
   (use Top/Front/Side view for best results).
2. Switch to **Orthographic** projection (**View → Standard views → Orthographic**).
3. Choose **Mesh → Meshes → Cutting → Cut by Polygon**.
4. Left-click to add polygon vertices in the 3-D view.
5. Right-click to close the polygon and apply the cut.
6. FreeCAD prompts: **Inner** (keep inside) or **Outer** (keep outside). Click the side to keep.

---

## Common mistakes and pitfalls

!!! warning "The cut is performed in 2-D screen space"
    The polygon is a vertical projection through the view direction. In
    perspective mode or with an off-axis viewpoint, the cut boundary will
    be oblique. Use **Orthographic** projection and align the view to a
    standard plane for predictable results.

!!! warning "Result has an open boundary"
    Cut by Polygon leaves an open mesh edge at the cut. Use
    [Fill Up Holes](fill-up-holes.md) to close it if needed.

---

## See also

- [Trim by Polygon](trim-by-polygon.md) — keep only what is inside the polygon
- [Trim by Plane](trim-by-plane.md) — trim with a 3-D plane
- [Fill Up Holes](fill-up-holes.md) — close the open boundary after cutting
- [Mesh Workbench](../index.md) — workbench overview
