# Roof

> **In one sentence:** Create a sloped roof surface from a closed wire outline,
> with configurable pitch angle and overhang per edge.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Roof  
**Command:** `Arch_Roof`  
**IFC class:** `IfcRoof`

Creates a roof surface from a closed wire defining the roof outline at eave
level. Each edge of the wire becomes one roof slope, with its own pitch angle
and eave overhang. Gable ends, hip ends, and valley ridges are computed
automatically.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Angles` | List of pitch angles, one per edge (degrees) |
| `Run` | Horizontal run distance per edge (mm) |
| `IdRel` | Ridge-related edge index (for hip roofs) |
| `Thickness` | Roof slab thickness (mm) |
| `Overhang` | Eave overhang distance (mm) |

---

## Common mistakes and pitfalls

!!! warning "Roof pitch looks wrong"
    The `Angles` property is a list — one value per edge of the wire. If fewer
    angles are provided than edges, the last angle repeats. Verify that
    `len(Angles) == len(wire.Edges)` and set individual angles in the
    Properties panel.

---

## See also

- [Slab](slab.md) — flat roof alternative
- [Level](level.md) — the storey the roof covers
- [BIM Workbench](../index.md) — workbench overview
