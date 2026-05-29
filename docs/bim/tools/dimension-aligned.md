# Dimension (Aligned)

> **In one sentence:** Place a dimension annotation that measures the true
> distance along the line between two selected points.

---

## Overview

**Workbench:** BIM  
**Menu:** Annotation → Dimension (Aligned)  
**Command:** `BIM_DimensionAligned`

Creates an aligned dimension — a `Draft::Dimension` object that measures
the straight-line distance between two points and displays the length with
a unit suffix. The dimension line runs parallel to the measured direction.

---

## Step-by-step

1. Activate **Annotation → Dimension (Aligned)**.
2. Click the first measurement point (vertex, edge midpoint, or free point).
3. Click the second measurement point.
4. Click where to place the dimension text offset.
5. The dimension is created.

---

## Common mistakes and pitfalls

!!! warning "Dimensions appear very small"
    Dimensions scale with the working plane. At building scale, set
    `FontSize` and `ArrowSize` in the annotation style to suitable
    building-scale values.

---

## See also

- [Dimension (Horizontal)](dimension-horizontal.md) — horizontal component only
- [Dimension (Vertical)](dimension-vertical.md) — vertical component only
- [Leader](leader.md) — leader line with text
- [BIM Workbench](../index.md) — workbench overview
