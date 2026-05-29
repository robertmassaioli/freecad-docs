# Mixed Curve

> **In one sentence:** Builds a 3-D curve as the intersection of two curves
> projected from different directions — plan and elevation views define the
> 3-D path.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Mixed Curve  
**Command:** `mixed_curve`

Given two curves projected from different directions (typically front-view and
side-view of a desired 3-D curve), the tool computes the 3-D curve at their
intersection. Classic technique used in ship-hull and aircraft-fuselage design.

---

## Step-by-step

1. Create two curves from different projection directions (e.g. a front-view
   outline and a side-view outline).
2. Select both objects.
3. Choose **Curves → Mixed Curve**.
4. The 3-D intersection curve appears.

---

## See also

- [Gordon Surface](gordon-surface.md) — use Mixed Curves as network inputs
- [Curves Workbench](../index.md) — workbench overview
