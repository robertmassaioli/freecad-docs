# Face Milling

> **In one sentence:** Generate a surfacing pass to flatten the top face
> of the stock and establish a consistent Z datum.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Face Milling  
**Command:** `CAM_MillFace`  
**Shortcut:** none

Generates a facing toolpath that passes back and forth across the top of the
stock (or selected faces) to remove stock material and produce a flat, consistent
surface. Typically the first operation in a milling programme.

---

## Intuition

Before machining a part, the top of the stock is often uneven — slightly bowed,
with mill marks, or at an imprecise height. Face Milling skims the top to give
a flat, clean datum that all subsequent depth measurements are referenced from.

---

## When to use it

- As the first operation in a milling job to establish a flat Z datum.
- When the stock top surface needs to be cleaned up before profiling or pocketing.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| BoundBox | Which faces to surface: selected faces, job stock, or all |
| Pattern | ZigZag, Offset, or Line |
| Step Over Percent | Radial step-over between passes (% of tool diameter) |
| Finish Depth | Extra depth beyond the face to ensure a clean cut |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## Common mistakes and pitfalls

!!! warning "Face Milling cuts too deep"
    If Final Depth is set below the model top face, the facing pass will cut
    into the part. Set Final Depth to skim only the stock surface above the
    model's top.

---

## See also

- [New Job](new-job.md) — face milling is typically the first operation added
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
