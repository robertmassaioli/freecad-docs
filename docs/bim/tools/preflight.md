# Preflight

> **In one sentence:** Run a set of validation checks on the BIM model before
> IFC export to catch common data errors.

---

## Overview

**Workbench:** BIM  
**Menu:** Manage → Preflight  
**Command:** `BIM_Preflight`

Runs a set of automated validation checks and reports any issues before IFC
export. Failed checks are listed with a link to the offending element so you
can fix them before exchange.

---

## Checks performed

| Check | What it verifies |
|-------|-----------------|
| IFC hierarchy | Every element is inside a valid Site → Building → Storey container |
| IFC classes | No elements have the generic default `IfcBuildingElement` class |
| Solid geometry | All elements produce valid closed solids |
| Openings | All doors and windows are hosted by a wall |
| Materials | All structural elements have a material assigned |

---

## Common mistakes and pitfalls

!!! warning "IFC export produces empty file"
    Failing the **IFC hierarchy** check is the most common cause. Place all
    elements inside a proper Site → Building → Level hierarchy and re-run
    Preflight.

---

## See also

- [IFC Elements](ifc-elements.md) — fix IFC class assignments
- [IFC Properties](ifc-properties.md) — fix missing property sets
- [Site](site.md) / [Building](building.md) / [Level](level.md) — set up the hierarchy
- [BIM Workbench](../index.md) — workbench overview
