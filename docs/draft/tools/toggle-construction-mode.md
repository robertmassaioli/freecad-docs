# Toggle Construction Mode

> **In one sentence:** Toggles construction geometry mode — when active, all
> newly-created Draft objects go into a special Construction group coloured blue.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Utilities → Toggle Construction Mode  
**Command:** `Draft_ToggleConstructionMode`

When construction mode is active:
- A green indicator appears in the Draft toolbar.
- All new Draft objects are added to the "Construction" group.
- Objects receive the construction colour (default: blue).

Construction objects serve as geometric references and are normally hidden
before export.

---

## Common mistakes and pitfalls

!!! warning "Construction objects remain in the document"
    Toggling off construction mode does not delete construction objects —
    they remain in the model tree. Toggle the visibility of the Construction
    group to hide them, or select and press Delete to remove them.

---

## See also

- [Add to Construction](add-to-construction.md) — move existing objects to construction group
- [Toggle Grid](toggle-grid.md) — show/hide the drafting grid
- [Draft Workbench](../index.md) — workbench overview
