# Repair Dimension References

> **In one sentence:** Re-links dimensions that have turned orange (broken)
> after a model topology change — lets you re-select the correct geometry
> for each broken dimension.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Dimensions → Repair Dimension References

When a model undergoes topology changes (a face is split, a feature is
deleted and re-added, or a body is Boolean-combined), the internal edge and
vertex IDs can change. Dimensions that referenced the old IDs turn orange
and display `?` instead of a value.

---

## Step-by-step

1. Observe which dimensions have turned orange and display `?`.
2. Choose **TechDraw → Dimensions → Repair Dimension References**.
3. A dialog lists all broken dimensions.
4. For each broken dimension, re-select the correct geometry in the view.
5. Click **OK**.

---

## Preventing broken dimensions

- Avoid renaming or deleting features that dimensions reference.
- Use **PartDesign → Named constraints** as reference geometry — named
  geometry is more stable across topology changes than raw edge/vertex IDs.
- After significant model restructuring, run Repair Dimension References
  before delivering the drawing.

---

## Common mistakes and pitfalls

!!! warning "Orange dimensions indicate broken references"
    Immediately after a model change, check whether any dimensions have turned
    orange. Orange means the referenced geometry no longer exists or has
    changed ID. Use Repair Dimension References before delivering the drawing.

---

## See also

- [Insert Dimension](dim-insert.md) — create new dimensions
- [TechDraw Workbench](../index.md) — workbench overview
