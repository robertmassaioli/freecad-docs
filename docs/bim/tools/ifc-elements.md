# IFC Elements

> **In one sentence:** Open a spreadsheet panel to view and change the IFC
> class assigned to every document object.

---

## Overview

**Workbench:** BIM  
**Menu:** Manage → Manage IFC Elements  
**Command:** `BIM_IfcElements`

Opens a spreadsheet-style panel listing all document objects with their
FreeCAD label, assigned IFC class, IFC predefined type, and material.
Changing an IFC class here changes what type the element exports as in IFC.

---

## Columns in the panel

| Column | Description |
|--------|-------------|
| Label | The object's FreeCAD name |
| IFC Class | The assigned IfcProduct subtype (e.g. `IfcWall`, `IfcColumn`) |
| IFC Type | Predefined type (e.g. `STANDARD`, `SHEAR`) |
| Material | Assigned material name |

---

## Changing an IFC class

Click the **IFC Class** cell for any object and choose from the dropdown.
This affects how downstream BIM applications (structural, MEP, FM) interpret
the element.

!!! tip
    Assigning the correct IFC class is critical for interoperability. A wall
    exported as `IfcSlab` will be treated as a horizontal element by structural
    analysis software — with incorrect load assumptions.

---

## See also

- [IFC Properties](ifc-properties.md) — manage `Pset_*` property sets
- [IFC Quantities](ifc-quantities.md) — manage quantity sets
- [Preflight](preflight.md) — validate before export
- [BIM Workbench](../index.md) — workbench overview
