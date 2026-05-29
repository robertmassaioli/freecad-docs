# Slab

> **In one sentence:** Create a horizontal floor or ceiling slab by
> extruding a planar shape by the slab thickness.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Slab  
**Command:** `BIM_Slab`  
**IFC class:** `IfcSlab`

Creates a horizontal floor or ceiling slab from a planar shape (face or closed
wire). The shape is offset vertically by the slab thickness, producing a solid
element.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Thickness` | Slab thickness (mm) |
| `Normal` | Direction of thickness growth (default: vertical, upward) |

---

## When to use it

- Concrete floor slabs between storeys.
- Roof slabs and flat roofs.
- Foundation slabs.

## When NOT to use it

- **Pitched roofs** — use [Roof](roof.md) for sloped roof surfaces.
- **Thin cladding panels** — use [Panel](panel.md) instead.

---

## See also

- [Column](column.md) — vertical structural support
- [Beam](beam.md) — horizontal structural beam
- [Roof](roof.md) — sloped roof surface
- [BIM Workbench](../index.md) — workbench overview
