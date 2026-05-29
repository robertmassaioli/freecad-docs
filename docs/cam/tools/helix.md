# Helix

> **In one sentence:** Generate a helical descending path into a circular
> hole or feature, spiralling down to final depth instead of plunging straight.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Helix  
**Command:** `CAM_Helix`  
**Shortcut:** none

Generates a helical interpolation path (circular in XY, descending in Z) to
machine or clear a circular feature. The tool spirals inward or outward as
it descends.

---

## Intuition

Plunging straight down into a circular hole at full depth puts maximum load on
the end mill tip. A helical entry spreads the load over the descent arc,
reducing heat, extending tool life, and producing a better surface finish in
the bore.

---

## When to use it

- Creating a circular pocket or bore with an end mill (no drilling).
- Entry into a pocket using a helical ramp instead of a plunge.
- Interpolating a hole larger than the tool diameter.

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Direction | Climb (CW) or conventional (CCW) |
| Step Over | Radial distance between helix passes |
| Keep Tool Down | If True, do not lift the tool between passes |

See [Common Parameters](common-parameters.md) for depth and height parameters.

---

## See also

- [Drilling](drilling.md) — point drilling for circular holes
- [Dressup — Ramp Entry](dressup-ramp-entry.md) — add helical entry to any operation
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
