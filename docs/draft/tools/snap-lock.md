# Snap Lock

> **In one sentence:** Master toggle for all Draft snapping — when off, all
> snap types are disabled and the cursor moves freely.

---

## Overview

**Workbench:** Draft  
**Toolbar:** Draft Snap  
**Command:** `Draft_Snap_Lock`  
**Shortcut:** `S` (while a drawing tool is active)

When Snap Lock is **off**, every individual snap type is effectively disabled
regardless of its own toggle state. This is the fastest way to temporarily
draw without snapping.

---

## How snapping works

When a Draft drawing tool is active:

1. FreeCAD tests each enabled snap type against nearby geometry.
2. The first matching snap is highlighted (coloured marker appears).
3. Clicking places the point at the snapped position.
4. Hold **Shift** to temporarily suppress all snapping for one click.

---

## Common mistakes and pitfalls

!!! warning "Snapping fails entirely when Snap Lock is off"
    If all snapping stops working, check the Snap Lock padlock icon in the
    toolbar. The `S` shortcut is easy to hit accidentally during a drawing
    session.

---

## See also

- [Show Snap Bar](show-snap-bar.md) — restore the snap toolbar if closed
- [Snap Endpoint](snap-endpoint.md) — most-used individual snap type
- [Draft Workbench](../index.md) — workbench overview
