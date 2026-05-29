# Chain Dimensions

> **In one sentence:** Creates a series of consecutive dimensions end-to-end
> across multiple features — horizontal, vertical, or oblique.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Format/Organize Dimensions

Three tools:

| Tool | Description |
|------|-------------|
| Create Horizontal Chain Dimension | Dimension a series of features end-to-end horizontally |
| Create Vertical Chain Dimension | Dimension a series of features end-to-end vertically |
| Create Oblique Chain Dimension | Dimension along an oblique direction |

A **chain dimension** (also called running dimension) places consecutive
dimensions so the end of one is the start of the next.

---

## Step-by-step (Horizontal Chain)

1. Select three or more vertices in a view (start point + one vertex per
   feature gap).
2. Choose **Create Horizontal Chain Dimension**.
3. Set the spacing (distance from the part to the first dimension line).
4. Click **OK**. One dimension is created per interval.

---

## Common mistakes and pitfalls

!!! warning "Chain dimensions accumulate tolerance"
    Chain dimensioning (end-to-end) accumulates manufacturing tolerance at
    each step. For precision features, use Coordinate Dimensions (baseline)
    instead. This is a fundamental GD&T principle.

---

## See also

- [Coordinate Dimensions](coordinate-dimensions.md) — baseline ordinate (no tolerance accumulation)
- [TechDraw Workbench](../../index.md) — workbench overview
