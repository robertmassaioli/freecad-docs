# Zebra Tool

> **In one sentence:** Displays a zebra stripe pattern on selected faces to
> visually detect surface continuity — G0 shows broken stripes, G1 shows
> continuous, G2 shows kink-free.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Zebra Tool  
**Command:** `ZebraTool`

Zebra stripes are rendered as if reflected from an infinite striped light
source. They are the standard visual technique for detecting continuity
discontinuities at surface junctions.

---

## Reading the stripe pattern

| Pattern at junction | Continuity level |
|--------------------|------------------|
| Stripes align but are broken (step at junction) | G0 (position only) |
| Stripes align and are continuous | G1 (tangent) |
| Stripes align, continuous, and kink-free | G2 (curvature) |

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| **Stripe width** | Controls the width of each stripe |
| **Rotation** | Rotates the stripe pattern orientation |

---

## Common mistakes and pitfalls

!!! warning "Stripes flicker when rotating the view"
    The stripe pattern is view-dependent — it is computed for the current
    camera angle. This is expected behaviour. Pause rotation to read the
    pattern, or switch to a fixed orthographic view.

---

## See also

- [Reflect Lines](reflect-lines.md) — more sensitive continuity check
- [Surface Analysis](surface-analysis.md) — curvature colour map
- [Comb Plot](comb-plot.md) — curvature display on curves
- [Curves Workbench](../index.md) — workbench overview
