# Insert Hole / Shaft Fit

> **In one sentence:** Places an ISO 286 hole-and-shaft tolerance annotation
> (e.g. **H7/g6**) on a dimension or view, with optional deviation values.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Symbols → Insert Hole / Shaft Fit

The annotation shows the tolerance designation and optionally the calculated
upper and lower deviations based on the nominal diameter and grade.

---

## ISO tolerance system basics

ISO 286 defines fits using:
- A **fundamental deviation letter** — uppercase for holes (A–ZC), lowercase
  for shafts (a–zc)
- An **IT grade number** (1–18) — lower numbers are tighter

Common fits:

| Designation | Type | Example use |
|-------------|------|-------------|
| H7/g6 | Clearance fit | Sliding bearing |
| H7/k6 | Transition fit | Gear on shaft |
| H7/p6 | Interference fit | Press fit |

---

## Step-by-step

1. Select a dimension (e.g. Diameter Dimension on a hole) or a view.
2. Choose **TechDraw → Symbols → Insert Hole / Shaft Fit**.
3. In the task panel:
   - Select the **Hole Tolerance** (e.g. H7).
   - Select the **Shaft Tolerance** (e.g. g6).
   - Choose display format: symbol only, deviations only, or both.
4. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Tolerance deviations require correct nominal diameter"
    The deviation values (computed in μm) depend on the nominal size. If the
    nominal diameter on the drawing is wrong, the computed deviations will
    also be wrong. Verify the nominal dimension before applying the fit annotation.

---

## See also

- [Insert Surface Finish Symbol](symbol-surface-finish.md) — surface roughness
- [Insert Diameter Dimension](dim-diameter.md) — the dimension this symbol attaches to
- [TechDraw Workbench](../index.md) — workbench overview
