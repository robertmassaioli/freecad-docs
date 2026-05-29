# Schedule

> **In one sentence:** Generate a quantity take-off table that aggregates
> BIM properties across all elements — door schedules, material quantities,
> equipment lists.

---

## Overview

**Workbench:** BIM  
**Menu:** Manage → Schedule  
**Command:** `Arch_Schedule`

Creates a **quantity take-off schedule** — a spreadsheet-like document that
aggregates properties from BIM elements by filter criteria. Schedules update
automatically when the model changes.

---

## Typical schedule types

- **Door schedule** — all IfcDoor objects with their dimensions, fire rating,
  hardware type, and room connections.
- **Window schedule** — all IfcWindow objects with dimensions and glazing type.
- **Material quantity schedule** — total wall area by material type.
- **Equipment list** — all IfcFurnishingElement objects with room location.

---

## Configuring a schedule

1. Run **Manage → Schedule**.
2. Add rows. Each row specifies:
   - A **filter** — which IFC class or property set value to match.
   - A **property** — which value to aggregate (`Area`, `Volume`, `Label`, etc.).
   - An **operation** — sum, count, or list.
3. Click OK. The schedule populates automatically.
4. The schedule updates when model elements change.

---

## See also

- [IFC Elements](ifc-elements.md) — assign IFC classes so schedules filter correctly
- [IFC Properties](ifc-properties.md) — add `Pset_*` properties that schedules can query
- [Space](space.md) — spaces contribute area data to room schedules
- [BIM Workbench](../index.md) — workbench overview
