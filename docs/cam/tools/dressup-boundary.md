# Dress-up: Boundary

> **In one sentence:** Clip an operation's toolpath to a user-defined boundary
> shape, removing all path segments outside the region.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Path Boundary  
**Command:** `CAM_DressupPathBoundary`  
**Shortcut:** none

Clips the output path of an operation to a specified boundary shape. Path
segments outside the boundary are removed; path segments that cross the
boundary edge are trimmed at the intersection.

---

## Intuition

Sometimes you want to run an operation (like a large Adaptive Clearing or
Surface pass) but only within a specific region — without creating a new
face selection. Boundary lets you define the region as a shape object and
clip any operation to it, without changing the base operation.

---

## Boundary sources

| Source | Description |
|--------|-------------|
| Selection | Use the selected shape or face as the boundary |
| Model | Use the job model's bounding box |
| Stock | Use the stock shape |

---

## When to use it

- Restricting a large Adaptive or Surface operation to a sub-region.
- Keeping lead-in arcs from exiting the stock boundary.
- Isolating a specific zone on a complex part for a different operation.

---

## Common mistakes and pitfalls

!!! warning "Boundary clips too aggressively"
    If the boundary is too tight, it may clip entry and exit moves, leaving
    the tool plunging into uncleared material. Add a small offset to the
    boundary shape.

---

## See also

- [Dressup: Lead In/Out](dressup-lead-inout.md) — arc entry/exit that may need boundary clipping
- [Adaptive Clearing](adaptive-clearing.md) — large-area operation often used with Boundary
- [Surface](surface.md) — 3D surface operation often used with Boundary
- [CAM Workbench](../index.md) — workbench overview
