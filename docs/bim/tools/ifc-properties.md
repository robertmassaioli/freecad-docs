# IFC Properties

> **In one sentence:** Attach and edit IFC property sets (Pset_*) on
> document objects for full IFC data compliance.

---

## Overview

**Workbench:** BIM  
**Menu:** Manage → Manage IFC Properties  
**Command:** `BIM_IfcProperties`

IFC property sets (`Pset_*`) are structured attribute groups defined by the
IFC schema and attached to elements. The IFC Properties panel lets you view
which property sets are attached, add new ones, and edit property values.

---

## Common property sets

| Pset | Applies to | Key properties |
|------|-----------|----------------|
| `Pset_WallCommon` | IfcWall | `IsExternal`, `ThermalTransmittance`, `LoadBearing` |
| `Pset_SlabCommon` | IfcSlab | `IsExternal`, `LoadBearing` |
| `Pset_ConcreteElementGeneral` | Concrete elements | `ConstructionMethod` |
| `Pset_DoorCommon` | IfcDoor | `IsExternal`, `AcousticRating`, `FireRating` |

---

## Common mistakes and pitfalls

!!! warning "Custom FreeCAD properties are not exported as IFC Psets"
    Properties added directly via the FreeCAD Properties panel are **not**
    IFC property sets. They will not appear in IFC export. Use this panel
    to attach official `Pset_*` sets.

---

## See also

- [IFC Elements](ifc-elements.md) — manage IFC class assignments
- [IFC Quantities](ifc-quantities.md) — manage quantity sets
- [Preflight](preflight.md) — validate before export
- [BIM Workbench](../index.md) — workbench overview
