# Dress-up: Lead In/Out

> **In one sentence:** Add tangential arc entries and exits to a profile pass
> to avoid dwell marks and over-cuts at the start and end points.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Lead In/Out  
**Command:** `CAM_DressupLeadInOut`  
**Shortcut:** none

Adds a lead-in arc at the start of a profile pass and a lead-out arc at the
end. The arcs approach and depart the profile tangentially, so the tool
gradually engages the cut rather than plunging perpendicular to the surface.

---

## Intuition

If a profile starts by plunging straight into the material, the tool dwells
momentarily at full engagement — which leaves a dwell mark or small gouge.
A lead-in arc brings the tool into the cut from the side, distributing the
entry load and avoiding the mark. The lead-out arc prevents the tool from
dwelling as it exits.

---

## When to use it

- Finishing passes where surface quality matters.
- Profile operations on materials sensitive to dwell marks (aluminium, brass).
- Any profile where the start/end point would otherwise be visible.

## When NOT to use it

- Rough passes where surface finish is not important.
- Operations where space is too tight for a lead arc (small pockets).

---

## Key parameters

| Parameter | Description |
|-----------|-------------|
| Lead In Length | Length of the lead-in arc (mm) |
| Lead Out Length | Length of the lead-out arc (mm) |
| Lead In Arc | Sweep angle of the lead-in arc (degrees) |
| Lead Out Arc | Sweep angle of the lead-out arc (degrees) |
| Start Radius | Radius of the lead-in arc |
| End Radius | Radius of the lead-out arc |
| Use Lead In / Use Lead Out | Enable or disable each arc individually |

---

## Common mistakes and pitfalls

!!! warning "Lead-in arc cuts outside the stock"
    Reduce Lead In Length, or apply a [Boundary](dressup-boundary.md)
    dress-up to clip the lead arc to the stock boundary.

---

## See also

- [Dressup: Ramp Entry](dressup-ramp-entry.md) — replace plunge entries with a ramp
- [Dressup: Boundary](dressup-boundary.md) — clip a path to a boundary region
- [Profile](profile.md) — the operation this dress-up is typically applied to
- [CAM Workbench](../index.md) — workbench overview
