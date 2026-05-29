# Snap Intersection

> **In one sentence:** Snaps to the intersection of two edges, including
> virtual intersections where edge extensions would meet.

---

## Overview

**Workbench:** Draft  
**Toolbar:** Draft Snap  
**Command:** `Draft_Snap_Intersection`

**Indicator:** White cross at the intersection.

Move the cursor near an intersection point — FreeCAD computes the crossing
of the two edges even if they do not physically touch.

---

## Virtual intersections

Intersection snap can find the point where two edge extensions would meet
(even beyond the edges' actual endpoints). Move the cursor over one edge,
then hover toward the intersection area while keeping the first edge highlighted.

---

## Common mistakes and pitfalls

!!! warning "Both edges must be visible"
    Intersection snap only works when both edges are detected. If an edge is
    behind another object, the intersection snap may not fire. Rotate the
    view or zoom in.

---

## See also

- [Snap Endpoint](snap-endpoint.md) — endpoints of individual edges
- [Snap Perpendicular](snap-perpendicular.md) — perpendicular foot on an edge
- [Snap Lock](snap-lock.md) — master snap toggle
- [Draft Workbench](../index.md) — workbench overview
