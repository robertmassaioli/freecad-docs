# Insert Welding Symbol

> **In one sentence:** Creates an ISO 2553 / AWS A2.4 welding symbol with
> reference line, arrow, weld symbols, and optional tail text — directly on
> a drawing view.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Symbols → Insert Welding Symbol

The welding symbol is a drawing object attached to a view via a leader line.
It does not modify the 3-D model.

---

## Step-by-step

1. Select a view on the page (the view showing the joint to be welded).
2. Choose **TechDraw → Symbols → Insert Welding Symbol**.
3. The Welding Symbol task panel opens. Fill in the fields:

   | Field | Description |
   |-------|-------------|
   | Arrow Side Symbol | Weld type on the arrow side (fillet, groove, plug, etc.) |
   | Other Side Symbol | Weld type on the other side |
   | Arrow Side Size | Weld size on the arrow side |
   | Other Side Size | Weld size on the other side |
   | Tail Text | Welding process or note (e.g. "SMAW" or "AWS D1.1") |
   | All Around | Toggle the all-around circle flag |
   | Field Weld | Toggle the field-weld flag |

4. Click **OK**. Drag the arrowhead to the weld location.

---

## Weld symbol types

| Symbol | Weld type |
|--------|-----------|
| Fillet | △ Fillet weld |
| Groove | ∨ or ∧ V-groove, bevel, U-groove |
| Plug | □ Plug or slot weld |
| Spot | ○ Spot or projection weld |
| Seam | = Seam weld |
| Back / Backing | ⌒ Backing or back weld |

---

## Common mistakes and pitfalls

!!! warning "Welding symbol requires a leader first"
    The Insert Welding Symbol tool works best when you first place a Leader
    Line (**TechDraw → Annotations → Insert Leader Line**) on the view. The
    weld symbol attaches to the leader's reference line.

---

## See also

- [Insert Leader Line](annotation-leader.md) — place the leader before inserting the symbol
- [Insert Surface Finish Symbol](symbol-surface-finish.md) — surface roughness symbol
- [TechDraw Workbench](../index.md) — workbench overview
