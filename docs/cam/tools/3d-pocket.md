# 3D Pocket

> **In one sentence:** Clear a pocket whose floor is a 3-D curved surface,
> adapting the path to the floor shape at each Z level.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → 3D Pocket  
**Command:** `CAM_Pocket3D`  
**Shortcut:** none

Generates a pocket clearing toolpath for pockets with non-horizontal floors.
At each Z level, the path adapts to the 3-D shape of the floor surface.

---

## Intuition

Standard [Pocket Shape](pocket-shape.md) assumes a flat floor. 3D Pocket
handles curved floors — like a pocket whose base follows a dome shape or a
ramp. The clearing path at each Z level follows the intersection of the floor
surface with that Z plane.

---

## When to use it

- Pockets with curved floors (domes, ramps, contoured bases).
- Clearing material above a 3-D surface feature.

## When NOT to use it

- **For flat-floor pockets**, use [Pocket Shape](pocket-shape.md) — it is
  faster and produces simpler paths.

---

## See also

- [Pocket Shape](pocket-shape.md) — flat-floor pocket clearing
- [Surface](surface.md) — full 3-D surface machining
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
