# Add Face Centerline

> **In one sentence:** Draws a centerline through the centre of a selected
> face — most commonly the axis of a cylindrical face or shaft.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Add Lines → Add Face Centerline

The centerline is drawn in the long-dash center-line ISO style and extends
beyond the face by a configurable overhang amount. It exists only on the
drawing — not in the 3-D model.

---

## Step-by-step

1. Click a face in a view (the cylindrical or flat face to centre).
2. Choose **TechDraw → Add Lines → Add Face Centerline**.
3. In the task panel, set:
   - **Orientation** — horizontal, vertical, or aligned to the face axis.
   - **Extension** — how far the centerline extends beyond the face edges.
4. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Extension adjustment"
    The default extension length is set in preferences. If centerlines look
    too short or too long for your standard, adjust the extension in the task
    panel or change the default in
    **Edit → Preferences → TechDraw → Annotation**.

---

## See also

- [Add Centerline Between 2 Lines](centerline-2lines.md) — symmetry axis between two edges
- [Add Centerline Between 2 Points](centerline-2points.md) — axis between two vertices
- [TechDraw Workbench](../index.md) — workbench overview
