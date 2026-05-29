# Pocket Shape

> **In one sentence:** Clear the interior of one or more closed boundaries
> in Z-level passes, stepping down until the final depth is reached.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Pocket Shape  
**Command:** `CAM_Pocket_Shape`  
**Shortcut:** none

Generates a 2.5D pocket clearing toolpath. The pocket interior is cleared in
passes at each Z level, working downward by Step Down until reaching Final Depth.

---

## Intuition

Pocket Shape is used whenever you need to remove all material inside a closed
boundary to a given depth — like machining a rectangular or contoured pocket in
a block. The tool zigzags, offsets, or follows another clearing pattern inside
the boundary, stepping down to the final depth.

---

## When to use it

- Machining pockets in a block (rectangular, circular, or arbitrary profile).
- Removing material inside a closed face boundary.
- Roughing a pocket before a Profile finish pass.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Pattern | Clearing pattern: ZigZag, Offset, ZigZagOffset, Line, Grid, Triangle |
| Angle | Direction of ZigZag lines (degrees from X axis) |
| Step Over Percent | Tool engagement as a percentage of tool diameter |
| Use Outline | If True, add a finishing pass around the pocket wall |
| Minimum Travel | Merge passes shorter than this distance |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## Common mistakes and pitfalls

!!! warning "Pocket leaves material in corners"
    A standard end mill cannot cut a true right-angle inside corner — it
    leaves a radius of material. Apply [Dressup — Dogbone](dressup-dogbone.md)
    to add corner reliefs, or use a smaller tool for a finish pass.

!!! warning "Step Down too large"
    An aggressive Step Down can break end mills. A safe starting value is
    50% of the tool diameter in soft materials.

---

## See also

- [Profile](profile.md) — follow a perimeter for profile finishing
- [Adaptive Clearing](adaptive-clearing.md) — high-efficiency trochoidal clearing
- [Dressup — Dogbone](dressup-dogbone.md) — add corner reliefs
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
