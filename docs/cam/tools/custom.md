# Custom

> **In one sentence:** Insert a block of arbitrary G-code text directly into
> the post-processed output.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Custom  
**Command:** `CAM_Custom`  
**Shortcut:** none

Inserts a user-supplied block of raw G-code into the output file at the
current position in the operation list. The text is passed through
unmodified — FreeCAD does not parse or validate it.

---

## Intuition

FreeCAD's standard operations cover the vast majority of machining scenarios,
but some machines or cycles fall outside what the built-in operations can
express. Custom lets you insert:

- Machine-specific sub-programmes or macros
- User-defined canned cycles not supported by the post-processor
- Probing or measurement routines
- G-code that exercises machine options (coolant, air blast, chip conveyor)

---

## When to use it

- Using a machine-specific canned cycle (e.g. thread milling macro).
- Inserting a G-code line FreeCAD does not generate (e.g. a specific M-code).
- Adding G-code from an external source into a FreeCAD programme.

## When NOT to use it

- For text comments — use [Comment](comment.md) instead.
- For programme pauses — use [Stop](stop.md) instead.

---

## Common mistakes and pitfalls

!!! warning "Custom G-code not validated"
    FreeCAD does not check the syntax or safety of Custom G-code. Incorrect
    code can crash the machine or damage the workpiece. Test custom blocks on
    a simulator first.

---

## See also

- [Comment](comment.md) — insert a text annotation (no machine effect)
- [Stop](stop.md) — insert a programme stop
- [Probe](probe.md) — insert a probing routine
- [CAM Workbench](../index.md) — workbench overview
