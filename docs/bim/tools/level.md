# Level

> **In one sentence:** Create a floor/storey container that groups all
> building elements at one floor elevation.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Level  
**Command:** `Arch_Level`  
**IFC class:** `IfcBuildingStorey`

A Level (Building Storey) represents one floor of the building. It has an
elevation property and contains all elements at that floor. Toggling a
Level's visibility in the model tree hides all elements on that floor —
useful for working on one storey at a time.

---

## Properties

| Property | Description |
|----------|-------------|
| `Elevation` | Height of this storey above the building base (mm) |
| `Height` | Floor-to-floor height (used for display and space calculation) |

---

## Visibility control

Toggling a Level's visibility in the document tree hides all elements it
contains. This lets you work on one storey at a time without visual clutter
from other floors.

---

## Common mistakes and pitfalls

!!! warning "Elements not appearing in the correct storey"
    Elements must be **children** of the Level in the document tree for
    IFC containment to be correct. Drag elements under the appropriate Level,
    or use **Manage → IFC Elements** to reassign containment.

---

## See also

- [Building](building.md) — the building container holding this level
- [Space](space.md) — room or zone inside this level
- [Site](site.md) — the top-level project container
- [BIM Workbench](../index.md) — workbench overview
