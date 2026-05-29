# Dress-up: Ramp Entry

> **In one sentence:** Replace straight plunge entries with a ramping or
> helical descent to reduce tool load at entry.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Ramp Entry  
**Command:** `CAM_DressupRampEntry`  
**Shortcut:** none

Replaces straight-down plunge moves at the start of each Z-level pass with a
ramping or helical entry. Plunging straight down is the worst-case load
condition for an end mill; ramping distributes this load over the approach
distance and is kinder to the tool and machine.

---

## Intuition

End mills are designed to cut laterally, not to drill straight down. A straight
plunge forces all the cutting load onto the tool tip. A ramp or helix entry
tilts the approach so the flutes engage from the side as the tool descends —
lower peak load, longer tool life, better surface finish at the entry point.

---

## Ramp methods

| Method | Description |
|--------|-------------|
| Ramp | Linear ZigZag ramp descending to the start depth |
| Helix | Helical spiral descending to the start depth |
| Plunge | Keep straight plunge (bypass — no change) |

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Ramp Method | Ramp, Helix, or Plunge |
| Angle | Ramp angle from horizontal (degrees) — smaller = gentler descent |
| Ramp Feed Rate | Override feed rate during the ramp portion |

---

## Common mistakes and pitfalls

!!! warning "Ramp produces incorrect depth"
    If the ramp angle is too steep relative to the operation's Step Down, the
    ramp can overshoot the target Z depth. Reduce the ramp angle or reduce
    the Step Down parameter.

!!! warning "Helix radius larger than feature"
    For a helix entry into a narrow slot or pocket, the helix radius may
    exceed the available space. Use a Ramp method instead, or reduce the
    helix radius.

---

## See also

- [Dressup: Lead In/Out](dressup-lead-inout.md) — tangential arc entry/exit
- [Profile](profile.md) — contour operation commonly paired with ramp entry
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
