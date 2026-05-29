# Wall

> **In one sentence:** Create a parametric vertical wall from a line, wire,
> or face with configurable thickness, height, and alignment.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Wall  
**Command:** `Arch_Wall`  
**IFC class:** `IfcWall` / `IfcWallStandardCase`

A Wall is a vertical or near-vertical partition. It follows a reference line
or wire, with thickness applied perpendicular to the path. Walls can host
doors and windows, be joined at corners, and carry material layers.

---

## Creating a wall

A Wall can be created from:

- **A line or wire** — wall follows the line/wire; thickness is applied perpendicular.
- **A face** — wall is extruded from the face normal.
- **Existing geometry** — FreeCAD wraps the shape.
- **Free-hand** — click two points in the 3-D view with no prior selection.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Width` | Wall thickness (mm). Default 200 mm. |
| `Height` | Wall height (mm). |
| `Align` | Which side of the reference line the thickness grows: Left, Right, or Center |
| `Normal` | Direction of height growth. Default (0,0,1) = vertical. |
| `Offset` | Shift from reference line along wall thickness direction |

---

## Compound walls and openings

- **Joining walls:** Use **Arch → Add** to join two walls at a T-junction or
  L-corner, creating proper topology.
- **Adding openings:** Place a [Door](door.md) or [Window](window.md) on the
  wall face — the wall automatically cuts an opening.
- **Manual subtraction:** Use **Arch → Remove** to subtract a shape from a
  wall as an opening.

---

## Common mistakes and pitfalls

!!! warning "Wall appears at wrong height"
    When working on an elevated storey, set the working plane to the storey
    elevation before drawing. Wall reference lines are drawn in world Z = 0
    unless the working plane is elevated.

---

## See also

- [Door](door.md) — insert a door into this wall
- [Window](window.md) — insert a window into this wall
- [Level](level.md) — the storey container for this wall
- [BIM Workbench](../index.md) — workbench overview
