# Pipeshell

> **In one sentence:** Creates a pipe-like surface by sweeping one or more
> profiles along a spine curve with explicit orientation control modes.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Pipeshell  
**Command:** `pipeshell`

Pipeshell gives more control over profile orientation than a standard BRep
`makePipe`.

---

## Orientation modes

| Mode | Description |
|------|-------------|
| **Fixed** | Profile maintains a fixed orientation in space |
| **Frenet** | Profile follows the curve's Frenet frame (may twist) |
| **Corrected Frenet** | Frenet with twist minimisation |
| **Binormal** | Profile normal tracks the curve's binormal |
| **Tangential** | Profile tangent aligned to rail tangent |
| **Auxiliary** | Orientation guided by a secondary rail or object |

---

## See also

- [Sweep 2 Rails](sweep-2-rails.md) — sweep between two explicit rail curves
- [Rotation Sweep](rotation-sweep.md) — sweep around a centre point
- [Curves Workbench](../index.md) — workbench overview
