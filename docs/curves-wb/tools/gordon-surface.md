# Gordon Surface

> **In one sentence:** Creates a surface that exactly interpolates a network
> of crossing curves — every U-direction and V-direction curve lies on the
> resulting surface.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Gordon Surface  
**Command:** `gordon`

A Gordon surface is the classic method for surfacing free-form shapes defined
by a network of cross-section and longitudinal guide curves. Every curve in
the network lies exactly on the surface.

---

## Input requirements

- At least 2 U-direction curves and 2 V-direction curves
- Each U curve must intersect each V curve exactly once
- The intersection points must be geometric (within tolerance)
- Use [Gordon Profile](gordon-profile.md) curves for best results

---

## Recommended workflow

1. Create U-family curves (cross-sections) using **Gordon Profile**.
2. Create V-family curves (longitudinal guides) using **Gordon Profile**.
3. Select all U curves, then all V curves.
4. Choose **Surfaces → Gordon Surface**.

---

## Common mistakes and pitfalls

!!! warning "Curves do not intersect"
    If the curves do not physically cross within the geometric tolerance,
    the tool fails. Use [Extend Curve](extend-curve.md) or adjust control
    points so curves cross exactly.

---

## See also

- [Gordon Profile](gordon-profile.md) — create the network input curves
- [IsoCurve](isocurve.md) — extract U/V iso-curves from an existing surface
- [Extend Curve](extend-curve.md) — ensure curves reach their intersections
- [Curves Workbench](../index.md) — workbench overview
