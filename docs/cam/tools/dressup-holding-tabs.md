# Dress-up: Holding Tabs

> **In one sentence:** Leave small bridges of uncut material in a profile path
> to keep the part from shifting or flying free during the final cut.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Tag  
**Command:** `CAM_DressupTag`  
**Shortcut:** none

Adds holding tabs — small bridges of uncut material — at selected points along
a Profile path. The tabs keep the part attached to the stock during the final
pass. After machining, the tabs are cut through manually with a chisel, saw,
or hand tool.

---

## Intuition

When profiling a part all the way through the stock, the final plunge cuts
the last connection and the part is suddenly free. On a router or high-speed
machine this can cause the part to move, gouge the surface, or become a
projectile. Holding tabs prevent this: the cutter rises at each tab location,
leaving a thin bridge.

---

## When to use it

- Full-depth profile cuts that separate a part from the stock sheet.
- Any profile operation where the part could shift during the final pass.
- Woodworking, sign-making, or sheet material routing.

## When NOT to use it

- Blind pockets that don't cut all the way through.
- Operations where manual tab removal would damage a precision surface.

---

## Adding tabs

1. Apply the Tag dress-up to a Profile operation.
2. In the task panel, click locations on the path preview where you want tabs.
3. Set tab Width, Height, and Angle.
4. Click OK.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Width | Tab width along the path direction |
| Height | Tab height — how much material is left uncut |
| Angle | Tab face angle (90° = vertical wall, <90° = chamfered face) |

---

## Common mistakes and pitfalls

!!! warning "Too few tabs — part shifts"
    Place tabs every 100–200 mm or at least 3 tabs per part. On asymmetric
    parts, place tabs to balance the holding force.

!!! warning "Tab height too small — tabs break during machining"
    Set tab height to at least 1–2 mm (or 10–20% of stock thickness for
    thin sheet material).

---

## See also

- [Dressup: Dogbone](dressup-dogbone.md) — corner reliefs for square-corner fits
- [Profile](profile.md) — the operation holding tabs are applied to
- [CAM Workbench](../index.md) — workbench overview
