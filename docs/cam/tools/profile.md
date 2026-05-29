# Profile

> **In one sentence:** Generate a contour-following toolpath along the
> edges or perimeter of selected faces, cutting on the inside or outside
> of the profile.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Profile  
**Command:** `CAM_Profile`  
**Shortcut:** none

Generates a 2.5D contour path following the outer or inner edge of a selected
face, set of edges, or the entire model boundary.

---

## Intuition

Profile is the most common 2D CAM operation — it follows a closed outline at
a specified Z depth, either outside the boundary (for external features) or
inside (for internal features like pockets with a finish pass). The tool moves
laterally at constant Z levels, stepping down by Step Down each pass.

---

## When to use it

- Cutting the outer profile of a part to size.
- Finish-passing the walls of a pocket after [Pocket Shape](pocket-shape.md).
- Following a specific face edge for trimming or profiling.

---

## Geometry selection

| Selection | Resulting path |
|-----------|----------------|
| One or more faces | Profile of each face's perimeter |
| One or more edges | Follow the edges directly |
| No selection | Profile of the entire model's outer boundary |

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Side | Which side of the edge to cut: Outside, Inside, Left, Right |
| Direction | CW or CCW cutting direction |
| Offset Extra | Additional radial offset from the geometry (positive = further away) |
| Use Compensation | Apply tool radius compensation (G41/G42) |
| Handle Multiple Features | Combine features into one pass or use separate passes |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## Common mistakes and pitfalls

!!! warning "Profile cuts on the wrong side"
    If the path is on the wrong side of the profile, change the **Side**
    parameter between Outside and Inside. For edge selections, try switching
    between Left and Right.

---

## See also

- [Pocket Shape](pocket-shape.md) — clear the interior of a profile
- [Dressup — Dogbone](dressup-dogbone.md) — add corner reliefs to inside corners
- [Dressup — Holding Tabs](dressup-holding-tabs.md) — prevent part from shifting during cut
- [Dressup — Lead In/Out](dressup-lead-inout.md) — smooth entry/exit arcs
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
