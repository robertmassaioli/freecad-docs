# Slot

> **In one sentence:** Cut a slot between two selected points or along
> a selected edge, stepping down in Z-level passes.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Slot  
**Command:** `CAM_Slot`  
**Shortcut:** none

Generates a slot cutting toolpath between two endpoints or along an edge.
The slot width equals the tool diameter; the depth is set by the depth
parameters.

---

## Intuition

Slot creates a simple, direct path from point A to point B at the tool
centre. Unlike Profile (which follows a perimeter) or Pocket (which fills
an area), Slot just cuts a groove. Use it for T-slots, keyways, and simple
linear cuts.

---

## When to use it

- Cutting a linear slot or keyway.
- Machining a T-slot or dovetail groove (with appropriate tool).
- Creating a simple groove along an edge.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Layer Mode | Single pass or multiple Z-level passes |
| Path Node Offset | Start/end offsets from the endpoint geometry |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## See also

- [Pocket Shape](pocket-shape.md) — clear an arbitrary pocket area
- [Profile](profile.md) — follow a perimeter profile
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
