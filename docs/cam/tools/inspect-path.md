# Inspect Path

> **In one sentence:** Display the raw internal G-code representation of a
> selected operation to debug unexpected toolpath behaviour.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Inspect Path  
**Command:** `CAM_Inspect`  
**Shortcut:** none

Displays the intermediate canonical G-code for the selected operation —
FreeCAD's internal representation before post-processing. This is always in
metric (mm) and uses absolute coordinates, regardless of the post-processor
settings.

---

## Intuition

When an operation produces an unexpected toolpath, the first diagnostic step
is to read the raw path commands. Inspect Path shows the exact G-code moves
that FreeCAD computed, which is separate from (and independent of) what the
post-processor will output.

If the canonical G-code looks correct but the post-processed output is wrong,
the issue is in the post-processor. If the canonical G-code looks wrong, the
issue is in the operation parameters.

---

## When to use it

- Debugging an operation that produces unexpected tool moves.
- Checking that a dress-up modified the path as expected.
- Verifying feed rate and spindle speed commands in the path.

---

## Step-by-step

1. Select an operation (or dress-up) in the Job's operation list.
2. Choose **CAM → Inspect Path**.
3. A text panel shows the raw G-code moves for that operation.

---

## Common mistakes and pitfalls

!!! warning "Canonical G-code is always metric and absolute"
    The output of Inspect Path uses mm and absolute G90/G21 — it does not
    reflect the post-processor's unit or coordinate mode settings.

---

## See also

- [Simulate](simulate.md) — visualise the toolpath in 3-D
- [Sanity Check](sanity-check.md) — automated validation
- [CAM Workbench](../index.md) — workbench overview
