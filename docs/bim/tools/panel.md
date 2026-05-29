# Panel

> **In one sentence:** Create a flat panel element such as a cladding panel,
> floor tile, or prefabricated wall panel.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Panel  
**Command:** `Arch_Panel`  
**IFC class:** `IfcPlate`

Creates a flat panel element — a thin planar solid. Used for cladding panels,
floor tiles, prefabricated wall panels, and similar thin elements that are
not structural walls or slabs.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Thickness` | Panel thickness (mm) |
| `Length` | Panel length if created without a base shape (mm) |
| `Width` | Panel width if created without a base shape (mm) |

---

## When to use it

- External cladding panels on a building facade.
- Prefabricated concrete or metal panel systems.
- Thin floor or ceiling tile elements.

## When NOT to use it

- **Structural walls** — use [Wall](wall.md) instead.
- **Glazed facade systems** — use [Curtain Wall](curtain-wall.md) instead.

---

## See also

- [Curtain Wall](curtain-wall.md) — grid-based glazed facade
- [Wall](wall.md) — structural or non-structural wall
- [BIM Workbench](../index.md) — workbench overview
