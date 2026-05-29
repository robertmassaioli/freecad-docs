# Blend Surface

> **In one sentence:** Creates a surface that smoothly connects two edges
> with a controlled continuity (G0, G1, or G2) at each boundary edge.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Blend Surface  
**Command:** `Curves_BlendSurf2`

The blend surface fills the gap between two adjacent faces, matching tangency
or curvature at each boundary.

---

## Continuity options

| Continuity | Condition imposed |
|------------|------------------|
| **G0** | Surface passes through the edge (position only) |
| **G1** | Surface is tangent to the adjacent face at the edge |
| **G2** | Surface matches the curvature of the adjacent face at the edge |

---

## Step-by-step

1. Select the two boundary edges (with their owner faces to define the
   continuity reference).
2. Choose **Surfaces → Blend Surface**.
3. Set continuity at each end in the task panel.

---

## See also

- [Blend Solid](blend-solid.md) — solid variant for adding material between faces
- [Blend Curve](blend-curve.md) — curve-level equivalent for edges
- [Zebra Tool](zebra-tool.md) — verify continuity of the resulting blend
- [Curves Workbench](../index.md) — workbench overview
