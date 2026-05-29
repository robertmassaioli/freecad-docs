# Door

> **In one sentence:** Insert a parametric door into a wall, automatically
> cutting an opening of the correct size.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Door  
**Command:** `BIM_Door`  
**IFC class:** `IfcDoor`

Inserts a parametric door into an existing wall. The door must be placed on
a wall face — it automatically cuts an opening through the wall. The door
shape is defined by a **preset** from the door library specifying panel type,
hinge side, and frame profile.

---

## Key properties

| Property | Description |
|----------|-------------|
| `Width` | Door leaf width (mm) |
| `Height` | Door opening height (mm) |
| `Frame` | Frame width (mm) |
| `Hosts` | The wall the door is inserted into |

---

## Step-by-step

1. Select the wall you want to insert the door into.
2. Go to **3D/BIM → Door**.
3. Click on the wall face to set the door position.
4. In the task panel, choose a preset, set width and height, and click OK.
5. The wall is automatically cut to create the opening.

---

## Common mistakes and pitfalls

!!! warning "Door does not cut through the wall"
    The door must be **placed on the wall face** while hosted by the wall.
    If the door is free-floating (not hosted), it creates no opening. Set
    the `Hosts` property to the wall, or select the wall first before running
    the Door command.

---

## See also

- [Window](window.md) — window opening in a wall
- [Wall](wall.md) — the wall that hosts the door
- [BIM Workbench](../index.md) — workbench overview
