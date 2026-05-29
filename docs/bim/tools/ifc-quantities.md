# IFC Quantities

> **In one sentence:** View and verify the IFC BaseQuantities computed for
> all elements before export.

---

## Overview

**Workbench:** BIM  
**Menu:** Manage → Manage IFC Quantities  
**Command:** `BIM_IfcQuantities`

IFC elements carry **BaseQuantities** — standardised geometric measurements
that receiving applications can use without recomputing from geometry. The
IFC Quantities panel shows the computed values for all elements and lets you
verify or override them before export.

---

## Standard quantity names

| IFC class | Standard quantities |
|-----------|---------------------|
| IfcSlab | `GrossArea`, `NetArea` |
| IfcWall | `GrossVolume`, `NetVolume` |
| IfcBeam | `Length` |
| IfcColumn | `Length` |

---

## See also

- [IFC Elements](ifc-elements.md) — manage IFC class assignments
- [IFC Properties](ifc-properties.md) — manage `Pset_*` property sets
- [Preflight](preflight.md) — validate model before export
- [BIM Workbench](../index.md) — workbench overview
