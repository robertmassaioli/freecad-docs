# Stairs

> **In one sentence:** Create a parametric stair flight with computed risers
> and treads from the storey height and user-specified dimensions.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Stairs  
**Command:** `Arch_Stairs`  
**IFC class:** `IfcStair`

Creates a parametric stair flight. FreeCAD computes the number of risers and
treads from the total rise height and user-specified step dimensions, following
standard stair geometry rules.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Width` | Stair flight width (mm) |
| `Height` | Total rise height (mm) — typically the storey height |
| `Nosing` | Tread nosing overhang (mm) |
| `TreadThickness` | Thickness of each tread (mm) |
| `RiserHeight` | Fixed or computed riser height |
| `NumberOfSteps` | Fixed or computed number of steps |
| `Landings` | Landing positions: None, At top, At bottom, At top and bottom |

---

## See also

- [Level](level.md) — storeys connected by stairs
- [Slab](slab.md) — landing slab geometry
- [BIM Workbench](../index.md) — workbench overview
