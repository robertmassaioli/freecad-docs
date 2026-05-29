# Window

> **In one sentence:** Insert a parametric window into a wall, automatically
> cutting an opening and placing glazing panels.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Window  
**Command:** `Arch_Window`  
**IFC class:** `IfcWindow`

Inserts a parametric window (or door) into a wall. Similar to [Door](door.md)
but with more glazing-panel presets. Windows can also be created from a sketch:
draw the opening profile on a wall face in the Sketcher, then run the Window
command.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Width` | Overall window width (mm) |
| `Height` | Overall window height (mm) |
| `Frame` | Frame width (mm) |
| `Louvre` | Louvre width (for louvre windows) |
| `Hosts` | The host wall |

---

## Common mistakes and pitfalls

!!! warning "Window does not cut through the wall"
    The window must be **hosted** by a wall. Select the wall first or set
    the `Hosts` property explicitly. A free-floating window does not create
    an opening.

---

## See also

- [Door](door.md) — door opening in a wall
- [Wall](wall.md) — the wall that hosts the window
- [BIM Workbench](../index.md) — workbench overview
