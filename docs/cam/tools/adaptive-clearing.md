# Adaptive Clearing

> **In one sentence:** Generate a trochoidal (curvilinear) clearing path
> that maintains constant tool engagement for high-efficiency milling.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Adaptive  
**Command:** `CAM_Adaptive`  
**Shortcut:** none

Generates an adaptive (trochoidal) clearing toolpath that keeps the tool
engagement angle constant throughout the cut. Also known as High-Efficiency
Milling (HEM) or trochoidal milling, this significantly extends tool life and
allows higher feed rates compared to conventional zigzag or offset pocketing.

---

## Intuition

Conventional pocket clearing varies tool engagement from 0% (air moves) to
100% (full-width slot cut). The variable load stresses the tool — particularly
at full engagement — and requires conservative feed rates.

Adaptive Clearing uses curved, looping paths that never exceed a set engagement
angle. The tool always takes the same chip load, allowing much higher feed rates
and longer tool life. The tradeoff is longer, more complex paths.

---

## When to use it

- Roughing pockets in tough materials (steel, stainless, titanium).
- When tool life is critical or tool breakage is a concern.
- When you have a fast spindle and want to maximise material removal rate.
- High-speed machining (HSM) toolpaths.

## When NOT to use it

- **Don't use it for finishing passes** — adaptive clearing is a roughing
  strategy. Use [Profile](profile.md) or [Pocket Shape](pocket-shape.md)
  for finishing.
- **Don't use it for simple 2D profiles** — the curved paths are unnecessary
  complexity for shallow, simple profiles.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Operation Type | Clearing (rough) or Profiling (finishing) |
| Step Over | Maximum radial engagement (% of tool diameter) |
| Lift Distance | How far the tool lifts between passes |
| Keep Tool Down Ratio | Ratio at which to keep the tool down vs lift |
| Force Inside-Out | Start cuts from the inside of the pocket |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## Common mistakes and pitfalls

!!! warning "Computation time can be long"
    Adaptive Clearing computes complex paths and can take significantly longer
    to calculate than Pocket Shape. For large pockets, allow extra time.

---

## See also

- [Pocket Shape](pocket-shape.md) — simpler zigzag/offset pocket clearing
- [Profile](profile.md) — perimeter finishing after adaptive clearing
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
