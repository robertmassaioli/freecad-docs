# Stop

> **In one sentence:** Insert a programme stop (M0 or M1) that pauses the
> machine and waits for the operator to press Cycle Start.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Stop  
**Command:** `CAM_Stop`  
**Shortcut:** none

Inserts a programme stop at the current position in the operation list.
The machine stops and waits for the operator to press Cycle Start before
continuing.

---

## Intuition

Placing a Stop between operations lets you inspect the workpiece mid-programme,
change fixturing, flip the part, or swap a tool that FreeCAD's tool controller
does not manage. It is the manual checkpoint in an otherwise automated programme.

---

## Stop types

| G-code | Name | Behaviour |
|--------|------|-----------|
| M0 | Programme Stop | Machine stops unconditionally |
| M1 | Optional Stop | Machine stops only if the controller's Optional Stop switch is active |

---

## When to use it

- Inspecting work-in-progress between roughing and finishing passes.
- Changing fixtures mid-programme.
- Allowing manual tool changes not managed by automatic tool change.
- Separating programme segments that require operator confirmation.

---

## See also

- [Comment](comment.md) — insert a text annotation (no machine effect)
- [Custom](custom.md) — insert arbitrary G-code
- [CAM Workbench](../index.md) — workbench overview
