# Comb Plot

> **In one sentence:** Displays a curvature comb on selected edges — spines
> perpendicular to the curve with length proportional to curvature — for
> diagnosing B-spline quality.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Curves → Comb Plot  
**Command:** `ParametricComb`

A curvature comb shows the curvature magnitude at every point along a curve
as perpendicular spines. Ideal combs are smooth and uniformly spaced; sharp
spikes indicate curvature peaks; oscillations indicate an over-controlled
B-spline.

Use Comb Plot before using a curve as a surface boundary — oscillating
curvature will produce oscillating surface quality.

---

## Reading the comb

| Comb appearance | Interpretation |
|-----------------|----------------|
| Smooth, uniform spines | Well-distributed curvature |
| Large spike | Local curvature peak — check for over-constrained knot |
| Alternating tall/short spines | Oscillating B-spline (too many control points) |
| Flat section (no spines) | Straight segment or inflection point |

---

## See also

- [Blend Curve](blend-curve.md) — create the smooth blends that produce clean combs
- [Zebra Tool](zebra-tool.md) — visual continuity check for surfaces
- [Curves Workbench](../index.md) — workbench overview
