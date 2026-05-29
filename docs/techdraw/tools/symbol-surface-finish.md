# Insert Surface Finish Symbol

> **In one sentence:** Places an ISO 1302 surface texture / roughness symbol
> on a view with Ra value, lay direction, and machining requirement.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Symbols → Insert Surface Finish Symbol

The symbol consists of the standard check-mark (√) with lines indicating
machining status, Ra roughness value, optional lay direction, and optional
machining allowance.

---

## Step-by-step

1. Select a view.
2. Choose **TechDraw → Symbols → Insert Surface Finish Symbol**.
3. In the task panel, fill in:

   | Field | Description |
   |-------|-------------|
   | Ra | Roughness average (e.g. 1.6, 3.2, 6.3 μm) |
   | Symbol Type | Basic / Machining required / No machining |
   | Lay Direction | = // ⊥ × M C R |
   | Method | Manufacturing method note |
   | Machining Allowance | Stock to remove (mm) |
   | Sampling Length | Sampling length for Ra measurement |

4. Click **OK**. Position on a face edge or with a leader.

---

## Ra standard values (ISO 1302)

| Ra (μm) | Typical process |
|---------|-----------------|
| 50 | Sand casting, rough cutting |
| 12.5 | Rough turning / milling |
| 6.3 | Normal turning / milling |
| 3.2 | Fine turning / milling |
| 1.6 | Grinding |
| 0.8 | Fine grinding |
| 0.4 | Honing / lapping |

---

## Common mistakes and pitfalls

!!! warning "Ra values are not enforced"
    FreeCAD accepts any numeric value for Ra — there is no enforcement of
    ISO preferred values. Always choose from the ISO 1302 preferred Ra series
    unless your standard explicitly uses non-standard values.

---

## See also

- [Insert Welding Symbol](symbol-welding.md) — welding annotation
- [Insert Hole / Shaft Fit](symbol-hole-shaft-fit.md) — ISO tolerance fit annotation
- [TechDraw Workbench](../index.md) — workbench overview
