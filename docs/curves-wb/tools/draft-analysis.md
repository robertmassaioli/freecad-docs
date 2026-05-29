# Draft Analysis

> **In one sentence:** Displays a false-colour map of draft angles — the
> angle between each face normal and the specified mould-release direction —
> to check for undercuts.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Draft Analysis  
**Command:** `Curves_DraftAnalysis`

Used to verify that a part can be demoulded without undercuts.

---

## Colour interpretation

| Colour | Meaning |
|--------|---------|
| **Green** | Sufficient positive draft — face releases cleanly |
| **Yellow** | Near-zero draft — marginal, may need adjustment |
| **Red** | Negative draft — undercut, part will not release |

---

## Step-by-step

1. Select a shape.
2. Choose **Surfaces → Draft Analysis**.
3. Set the **pull direction** in the task panel.
4. The colour map updates immediately.

---

## See also

- [Surface Analysis](surface-analysis.md) — general curvature colour map
- [Curves Workbench](../index.md) — workbench overview
