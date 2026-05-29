# IsoCurve

> **In one sentence:** Extracts iso-parameter curves from a face — lines of
> constant U or constant V — useful as guide rails or for inspecting surface
> quality.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → IsoCurve  
**Command:** `IsoCurve`

An iso-curve is a curve on a surface at a constant U or constant V parameter
value. Multiple iso-curves can be extracted simultaneously.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| **Direction** | U or V iso-curves |
| **Number of curves** | How many iso-curves to create |
| **Parameter value** | Single specific parameter to extract one iso-curve |

---

## Common uses

- Guide rails for a sweep
- Visual aids for inspecting surface quality
- Input curves for a Gordon surface

---

## See also

- [Gordon Surface](gordon-surface.md) — use iso-curves as U/V network input
- [Surface Analysis](surface-analysis.md) — colour-map surface curvature
- [Curves Workbench](../index.md) — workbench overview
