# Blend Curve

> **In one sentence:** Creates a B-spline that smoothly connects two edge
> endpoints with a specified geometric continuity (G1, G2, or G3) at each end.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Blend Curve  
**Command:** `ParametricBlendCurve`

The blend curve fills the gap between two edges, matching the continuity of
each source edge at their shared endpoints.

---

## Continuity options per end

| Continuity | Condition imposed |
|------------|------------------|
| **G1** | Tangent direction matches the source edge |
| **G2** | Curvature (magnitude + direction) matches the source edge |
| **G3** | Rate of curvature change matches the source edge |

The **Size** parameter controls how strongly the blend curve "pulls" toward
each source edge's tangent direction.

Double-click the resulting object to enter freehand mouse-editing mode, where
you can drag the size handles interactively.

---

## Common mistakes and pitfalls

!!! warning "Gaps at endpoints"
    If gaps are visible at the endpoints, the continuity order requested
    exceeds what the source curve can supply (e.g. requesting G2 from a G1
    source). Reduce the continuity order, or ensure the source edges have
    sufficient continuity at their endpoint.

---

## See also

- [Blend Surface](blend-surface.md) — surface equivalent for connecting two face edges
- [Comb Plot](comb-plot.md) — verify curvature continuity of the blend result
- [Curves Workbench](../index.md) — workbench overview
