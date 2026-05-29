# Toggle Active

> **In one sentence:** Enable or disable a selected operation so it is
> included or skipped during simulation and post-processing.

---

## Overview

**Workbench:** CAM  
**Menu:** Context menu → Toggle Active  
**Command:** `CAM_OpActiveToggle`  
**Shortcut:** none

Toggles the active state of the selected operation. A disabled operation is
skipped during post-processing and simulation but remains in the document tree
and can be re-enabled at any time.

---

## Intuition

During development, you often want to test one operation in isolation without
running the entire programme. Toggle Active lets you disable operations you
don't want to simulate or post-process right now, without deleting them.

Common uses: testing a finishing pass without re-running the roughing pass;
disabling decorative engraving during test cuts; temporarily skipping a
probing routine.

---

## When to use it

- Testing one specific operation in isolation during development.
- Temporarily excluding an operation from a test cut.
- Disabling an operation that is not needed for a particular fixturing setup.

---

## Step-by-step

1. Right-click the operation in the Job's operation list.
2. Choose **Toggle Active** from the context menu.
3. The operation icon changes to indicate its disabled state.
4. Run [Simulate](simulate.md) or [Post Process](post-process.md) — the
   disabled operation is skipped.

---

## See also

- [Simulate](simulate.md) — verify the result with selected operations active
- [Post Process](post-process.md) — post-process with selected operations active
- [CAM Workbench](../index.md) — workbench overview
