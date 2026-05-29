# Comment

> **In one sentence:** Insert a text comment into the G-code output with no
> effect on machine motion.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Comment  
**Command:** `CAM_Comment`  
**Shortcut:** none

Inserts a comment string into the G-code output at the current position in the
operation list. The comment appears in the generated file (e.g. `; Begin finish pass`)
and is ignored by the machine controller.

---

## Intuition

Comments are documentation in the G-code file. They help you (or the machine
operator) understand what each section of the programme is doing. Comments
have no effect on tool motion.

---

## When to use it

- Marking the start and end of each major programme section.
- Annotating tool change points.
- Adding metadata (part number, date, operator) to the G-code header.

---

## See also

- [Stop](stop.md) — insert a programme pause (M0/M1)
- [Custom](custom.md) — insert arbitrary G-code
- [CAM Workbench](../index.md) — workbench overview
