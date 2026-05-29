# Curtain Wall

> **In one sentence:** Create a non-structural glazed facade system from a
> base surface divided into a grid of mullions and panels.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Curtain Wall  
**Command:** `Arch_CurtainWall`  
**IFC class:** `IfcCurtainWall`

A Curtain Wall is a non-structural facade system made of a grid of panels
(typically glass). FreeCAD creates it from a base surface, dividing it into
a grid of mullions and panels at specified intervals.

---

## Key properties

| Property | Description |
|----------|-------------|
| `VerticalMullionNumber` | Number of vertical divisions |
| `HorizontalMullionNumber` | Number of horizontal divisions |
| `MullionSize` | Width of mullion members (mm) |
| `PanelThickness` | Thickness of infill panels (mm) |

---

## When to use it

- Modern glazed building facades.
- Non-structural external cladding systems.
- Anywhere a grid of panels covers a curved or flat surface.

## When NOT to use it

- Structural masonry or concrete walls — use [Wall](wall.md) instead.

---

## See also

- [Wall](wall.md) — structural or non-structural opaque wall
- [Panel](panel.md) — individual flat panel element
- [BIM Workbench](../index.md) — workbench overview
