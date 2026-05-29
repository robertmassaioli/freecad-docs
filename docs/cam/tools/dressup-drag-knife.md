# Dress-up: Drag Knife

> **In one sentence:** Adapt a toolpath for a drag knife cutter by adding
> small swivel arcs at sharp corners so the blade can rotate to the new direction.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Drag Knife  
**Command:** `CAM_DressupDragKnife`  
**Shortcut:** none

Modifies a toolpath to accommodate a **drag knife** — a blade that trails
behind the motion direction rather than cutting from the front like a rotating
mill. At each sharp corner, the path is extended with a small "swivel" arc
that allows the blade to pivot to the new cutting direction before the corner
is reached.

---

## Intuition

A drag knife blade offset from its pivot point. As the machine changes
direction, the blade needs to swing around before it can track the new
direction correctly — if the machine simply turns the corner, the blade drags
sideways and leaves a ragged cut. The swivel arc gives the blade room to pivot
while keeping the cutting point stationary.

---

## When to use it

- Vinyl cutting, foam cutting, or paper cutting with a drag knife tool.
- Any machine equipped with a trailing blade rather than a rotating spindle.

## When NOT to use it

- Rotating end mill or drill operations — swivel arcs make no sense with a
  spinning cutter.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Angle | Minimum corner angle (degrees) that triggers a swivel arc |
| Radius | Radius of the swivel arc |

---

## See also

- [Profile](profile.md) — the operation drag knife is typically applied to
- [CAM Workbench](../index.md) — workbench overview
