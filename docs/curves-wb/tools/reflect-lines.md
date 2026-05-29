# Reflect Lines

> **In one sentence:** Highlights reflection lines — curves where a parallel
> light source is specularly reflected toward the viewer — which are more
> sensitive than zebra stripes for detecting G2 discontinuities.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Reflect Lines  
**Command:** `ReflectLines`

Reflection lines update when the view rotates. They reveal subtle G2
discontinuities that zebra stripes might miss, because a tangent discontinuity
creates a clear kink in reflection lines.

---

## Step-by-step

1. Select a face or shell.
2. Choose **Surfaces → Reflect Lines**.
3. Reflection lines are computed for the current view direction.
4. Rotate the view to see how the lines behave across the surface.

---

## See also

- [Zebra Tool](zebra-tool.md) — simpler stripe-pattern continuity check
- [Surface Analysis](surface-analysis.md) — curvature colour map
- [Curves Workbench](../index.md) — workbench overview
