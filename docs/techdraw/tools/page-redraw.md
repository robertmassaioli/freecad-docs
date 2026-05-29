# Redraw Page

> **In one sentence:** Forces all views on the active drawing page to
> recompute their projected geometry — useful when a view appears stale after
> a model change.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Page → Redraw Page

TechDraw normally updates views automatically when the model recomputes.
Occasionally a view can become stale — showing old geometry — after a model
change that did not propagate fully. Redraw Page triggers a forced refresh
of every view on the active page.

---

## When to use it

- A view still shows old geometry after editing the 3-D model and running
  **Edit → Refresh** (Ctrl+R).
- A section or detail view fails to update after changing the parent view.
- Views appear blank or corrupted after undo/redo operations.

---

## See also

- [Insert Default Page](page-insert-default.md) — create a drawing page
- [TechDraw Workbench](../index.md) — workbench overview
