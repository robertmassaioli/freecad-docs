# Dress-up: Dogbone

> **In one sentence:** Add corner relief over-cuts at inside corners so that
> square-cornered parts can fit into pockets milled with a round end mill.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Dogbone  
**Command:** `CAM_DressupDogbone`  
**Shortcut:** none

Adds dogbone (or T-bone) corner reliefs at inside corners of a pocket or
profile path. A full-radius end mill cannot cut a true right-angle inside
corner — it leaves a fillet of material equal to the tool radius. A dogbone
relief over-cuts a small circle at each corner so that a square-cornered part
(e.g. a dovetail, tenon, or sliding fit) can seat fully.

---

## Intuition

When an end mill reaches an inside corner, it must round the corner to its
own radius. If a square-cornered mating part must fit into that pocket, it
will bind on these radii. A dogbone drill moves the tool slightly past the
corner, cutting a small circular relief — the "bone" of the dog shape —
that provides clearance for the square corner.

---

## When to use it

- Pocket or profile operations where a square-cornered part must fit.
- Joinery (dovetails, box joints, mortise and tenon) routed on a CNC.
- Any operation where corner radii would prevent assembly.

## When NOT to use it

- On outside corners — dogbone reliefs are only meaningful at inside corners.
- When the mating part has radiused corners that match the tool radius.

---

## Dogbone styles

| Style | Description |
|-------|-------------|
| Dogbone | Circular over-cut tangent to both wall faces |
| T-bone | T-shaped slot along one wall face |
| Horizontal | Extension perpendicular to the X-axis |
| Vertical | Extension perpendicular to the Y-axis |

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Incision | Relief cut size as a percentage of tool diameter |
| Custom | Fixed extra distance instead of percentage |
| Side | Which corners get relief: left, right, or both |

---

## Common mistakes and pitfalls

!!! warning "Dogbone applied to Pocket — corners still not clean"
    The Dogbone dress-up works on the path output, not the geometry. For a
    Pocket operation, the relief is only added to the final finishing pass.
    For a rough pocket interior, apply a Profile finish pass around the pocket
    perimeter and add Dogbone to that Profile.

---

## See also

- [Dressup: Holding Tabs](dressup-holding-tabs.md) — keep tabs of uncut material
- [Pocket Shape](pocket-shape.md) — the operation this is typically applied to
- [Profile](profile.md) — contour operation commonly paired with Dogbone
- [CAM Workbench](../index.md) — workbench overview
