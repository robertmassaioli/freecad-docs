# V-Carve

> **In one sentence:** Generate a variable-depth carving path using a V-bit
> where wider regions are cut deeper and narrower regions shallower.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → V-Carve  
**Command:** `CAM_Vcarve`  
**Shortcut:** none

Generates a variable-depth carving path using a V-bit. The V-bit's wedge
geometry naturally follows 2-D artwork — wider areas are cut deeper and
narrower areas shallower — producing crisp, tapered engraving characteristic
of traditional hand-carved lettering.

---

## Intuition

A V-bit is a wedge. When pushed into material, the width of the cut depends
on how deep the bit goes. V-Carve calculates the correct depth at every point
along the path so that the V-bit exactly fits within the artwork outline —
wider strokes receive a deeper pass, hairline strokes receive a shallow pass.

This is different from [Engrave](engrave.md), which ignores the tool shape
and traces at a fixed Z depth regardless of width.

---

## When to use it

- High-quality engraved lettering using a V-bit.
- Artistic carving of variable-width artwork or logos.
- Producing the traditional "chiselled" look in wood or metal.

## When NOT to use it

- **For constant-depth line tracing** — use [Engrave](engrave.md) instead.
- **For chamfering edges** — use [Deburr](deburr.md) instead.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Discretize | Number of sample points along each edge |
| Colinear Tolerance | Threshold for merging colinear path segments |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## See also

- [Engrave](engrave.md) — constant-depth edge/text tracing
- [Deburr](deburr.md) — chamfer/deburr around face edges
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
