# Insert Diameter Dimension

> **In one sentence:** Measures the diameter of a circle, placing the dimension
> line through the centre with arrowheads at both ends and a **⌀** prefix.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Dimensions → Insert Diameter Dimension

The ⌀ (diameter) symbol is added automatically to the prefix. The dimension
line passes through the circle centre.

---

## Step-by-step

1. Click a **full circle** edge in a view.
2. Choose **TechDraw → Dimensions → Insert Diameter Dimension**.
3. Click to place the dimension text.

---

## Common mistakes and pitfalls

!!! warning "Diameter symbol on radius dimensions"
    Insert Diameter Dimension adds the ⌀ prefix automatically. If you use
    Insert Length Dimension on a circle diameter instead, you will get a bare
    number — add the prefix manually via `FormatSpec = "⌀%.2f"`.

!!! warning "Cramped display on small circles"
    If the circle is small, the full diameter line through the centre may be
    cramped. Set `FlipArrowheads = true` to move arrowheads outside the circle,
    or switch to a Radius Dimension.

---

## See also

- [Insert Radius Dimension](dim-radius.md) — radius with R prefix
- [TechDraw Workbench](../index.md) — workbench overview
