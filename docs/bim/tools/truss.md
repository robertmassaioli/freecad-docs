# Truss

> **In one sentence:** Create a structural truss from a line, automatically
> generating chord, vertical, and diagonal members.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Truss  
**Command:** `Arch_Truss`  
**IFC class:** `IfcMember`

Creates a structural truss from a single line, automatically generating the
top chord, bottom chord, vertical members, and diagonal members based on the
selected truss pattern.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Height` | Truss depth (mm) |
| `TrussAngle` | Diagonal member angle (degrees) |
| `SlantType` | Diagonal pattern: Pratt, Warren, Howe, etc. |

---

## See also

- [Beam](beam.md) — simple structural beam
- [Frame](frame.md) — profile sweep for custom linear members
- [BIM Workbench](../index.md) — workbench overview
