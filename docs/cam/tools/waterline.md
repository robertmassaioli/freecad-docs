# Waterline

> **In one sentence:** Generate horizontal constant-Z passes that follow
> the model silhouette at each Z level — ideal for steep walls and undercuts.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Waterline  
**Command:** `CAM_Waterline`  
**Shortcut:** none

!!! note "Requires OpenCamLib"
    Waterline requires the OpenCamLib (OCL) library to be installed.

Generates horizontal (constant-Z) passes at each Z level that follow the
silhouette of the model — similar to contour lines on a topographic map.
Well-suited for steep walls and undercuts where planar scanning produces poor
surface finish.

---

## Intuition

The [Surface](surface.md) operation's planar scan moves the tool in long
parallel lines. On steep walls, this means the tool passes are very far apart
as measured on the surface (even though they are equally spaced in Z). Waterline
follows the model outline at each Z level, keeping a consistent step-over even
on vertical and near-vertical walls.

---

## When to use it

- Finishing steep walls and near-vertical surfaces.
- Complementing Surface with a scallop-free approach on steep geometry.
- Multi-pass finishing of tall, narrow features.

## When NOT to use it

- **Requires OpenCamLib** — unavailable without it.
- **For shallow, mostly-flat surfaces** — [Surface](surface.md) is more
  efficient there.

---

## See also

- [Surface](surface.md) — 3-D planar surface machining
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
