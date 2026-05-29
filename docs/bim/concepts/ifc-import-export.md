# IFC Import and Export

> **In one sentence:** IFC (Industry Foundation Classes) is the open standard
> format for exchanging BIM data between applications — understanding the
> difference between Classic and Native IFC modes, and how to prepare a model
> for clean exchange, determines whether other software can correctly read your
> FreeCAD model.

---

## What is IFC?

**IFC** (Industry Foundation Classes) is an open, neutral file format
maintained by buildingSMART International for sharing building and
infrastructure data. Unlike DXF (geometry only) or PDF (presentation only),
an IFC file carries:

- **Geometry** — the 3-D shape of each element.
- **Semantics** — what each element *is* (wall, column, door, slab, pipe).
- **Properties** — physical and functional data (fire rating, material, load
  bearing, thermal transmittance).
- **Relationships** — containment (element is in this storey), adjacency
  (wall meets wall), hosting (door is in this wall).
- **Quantities** — computed measurements (area, volume, length).

Common IFC versions in use: **IFC 2×3** (most widely supported),
**IFC 4** (current standard), **IFC 4.3** (infrastructure extension).

---

## Two IFC modes in FreeCAD

FreeCAD BIM supports two approaches to IFC. They are fundamentally different
in how the document relates to the IFC data.

### Classic IFC

| Step | What happens |
|------|-------------|
| Import | IFC entities are read and converted to FreeCAD objects |
| Edit | Work with FreeCAD-native objects (Wall, Column, etc.) |
| Export | FreeCAD objects are converted back to IFC and written to a file |

Classic IFC is useful for viewing and lightly editing imported models.
The conversion step means that some IFC data (custom property sets,
complex geometry types) may not round-trip perfectly.

### Native IFC (recommended)

| Step | What happens |
|------|-------------|
| Open / import | The IFC file is loaded as-is; every IFC entity becomes a FreeCAD object that *is* that IFC entity |
| Edit | Modifying geometry or properties modifies the underlying IFC entity directly |
| Save | Writes the modified IFC file back — no conversion step |

Native IFC preserves all IFC data including custom property sets, IFC GUIDs,
and complex geometry types. Use it for all new projects intended for BIM
collaboration.

To start a Native IFC project: **File → New** then use the **BIM** workbench
tools directly — the document is automatically kept in sync with the IFC model.
To open an existing IFC file natively: **File → Open** and select an `.ifc` file.

---

## The Site → Building → Level hierarchy

IFC requires every element to be contained in a **spatial structure**:

```
IfcProject
 └── IfcSite
      └── IfcBuilding
           └── IfcBuildingStorey (Level)
                └── IfcSpace (optional)
                     └── IfcWall / IfcColumn / IfcBeam / ...
```

This hierarchy is not optional — it is a requirement of the IFC schema.
An element that is not assigned to a storey will either fail Preflight
or be silently dropped by receiving applications.

**Create the hierarchy first** using
[Site](../tools/site.md), [Building](../tools/building.md), and
[Level](../tools/level.md) before adding walls and other elements.

---

## IFC class assignments

Every FreeCAD BIM object must have an IFC class assigned. The class
determines how receiving software interprets the element:

| FreeCAD object | Typical IFC class | Notes |
|----------------|------------------|-------|
| Wall | `IfcWall` or `IfcWallStandardCase` | Standard for straight walls |
| Column | `IfcColumn` | Vertical load-bearing member |
| Beam | `IfcBeam` | Horizontal load-bearing member |
| Slab | `IfcSlab` | Floor, ceiling, or roof plate |
| Door | `IfcDoor` | Must be hosted by a wall |
| Window | `IfcWindow` | Must be hosted by a wall |
| Equipment | `IfcBuildingElementProxy` | Generic catch-all for non-standard elements |

Change IFC class assignments using
[IFC Elements](../tools/ifc-elements.md) — the spreadsheet-style panel
that lists all objects with their current class.

!!! warning "Wrong IFC class causes incorrect behaviour in receiving software"
    A structural column exported as `IfcWall` will be treated as a wall by
    structural analysis software — with incorrect load path assumptions.
    A door exported as `IfcBuildingElementProxy` will not be recognised as an
    opening by energy analysis software.

---

## Property sets (Pset_*)

IFC elements carry structured attribute groups called **property sets**.
Standard property sets are defined by the IFC schema and named with the
`Pset_` prefix:

| Property set | Applies to | Key properties |
|-------------|-----------|----------------|
| `Pset_WallCommon` | IfcWall | `IsExternal`, `LoadBearing`, `ThermalTransmittance` |
| `Pset_SlabCommon` | IfcSlab | `IsExternal`, `LoadBearing` |
| `Pset_DoorCommon` | IfcDoor | `IsExternal`, `FireRating`, `AcousticRating` |
| `Pset_ConcreteElementGeneral` | Concrete | `ConstructionMethod` |
| `Pset_BuildingElementProxyCommon` | Proxy | `Reference` |

Attach and edit property sets using
[IFC Properties](../tools/ifc-properties.md).

!!! warning "FreeCAD Data-panel properties are NOT IFC property sets"
    Properties you add via the FreeCAD Properties panel (Data tab) are
    FreeCAD-internal properties. They are **not** written to IFC export.
    Use the IFC Properties panel to attach standard `Pset_*` sets.

---

## Quantities

IFC BaseQuantities are standardised geometric measurements (area, volume,
length) that receiving applications can use directly without recomputing from
geometry. FreeCAD computes these automatically.

Check computed quantities with
[IFC Quantities](../tools/ifc-quantities.md) before export.

---

## Export workflow

1. **Build the spatial hierarchy** — Site → Building → Level, then place
   all elements in a Level.
2. **Assign IFC classes** — use IFC Elements to verify every object has
   the correct class.
3. **Add required property sets** — use IFC Properties to attach `Pset_*`
   sets for fire rating, load bearing, etc.
4. **Run Preflight** — [Preflight](../tools/preflight.md) checks hierarchy,
   classes, solid geometry, hosted openings, and materials.
5. **Export** — **File → Export → IFC** (`.ifc` or `.ifczip`).

### Common Preflight failures and fixes

| Preflight check | Failure | Fix |
|-----------------|---------|-----|
| IFC hierarchy | Element not in a storey | Move element inside a Level object |
| IFC classes | Object has generic class `IfcBuildingElement` | Assign a specific class in IFC Elements |
| Solid geometry | Object produces an invalid mesh | Check the FreeCAD shape for errors; use Part → Check Geometry |
| Openings | Door/window not hosted by a wall | Select the door/window, then `BIM → Add/Remove host` to assign it to a wall |
| Materials | Structural element has no material | Assign a material via Manage → Material |

---

## Import workflow

1. **File → Open** and select an `.ifc` file (or `File → Import` to import
   into an existing document).
2. In the import dialog, choose **Native IFC** or **Classic IFC** mode.
3. For large files, use the **import options** to filter which storey or
   IFC class to import (avoids loading the entire model).
4. After import, inspect the model tree — each IfcBuildingStorey becomes a
   Level, each wall/column/beam becomes its corresponding BIM object.

!!! warning "Large IFC files can be slow to load"
    IFC files from other BIM applications often contain thousands of elements.
    Use the import filter to load only the storey or discipline you need.

!!! warning "Complex geometry may import as mesh"
    Swept profiles, computational geometry, and non-standard IFC geometry
    types that FreeCAD cannot parametrically reconstruct are imported as
    mesh representations. They are read-only (cannot be dimensionally edited).

---

## Python API

```python
import importIFC
import FreeCAD as App

# Import an IFC file (Classic IFC mode)
importIFC.open("/path/to/model.ifc")

# Export the current document to IFC
import exportIFC
doc = App.ActiveDocument
exportIFC.export(doc.Objects, "/path/to/output.ifc")
```

For Native IFC, the file is the document — use `App.openDocument()`:

```python
doc = App.openDocument("/path/to/native_model.ifc")
# Edit objects...
doc.save()   # writes back to the .ifc file
```

---

## See also

- [IFC Elements](../tools/ifc-elements.md) — manage IFC class assignments
- [IFC Properties](../tools/ifc-properties.md) — manage Pset_* property sets
- [IFC Quantities](../tools/ifc-quantities.md) — manage quantity sets
- [Preflight](../tools/preflight.md) — validate before export
- [Site](../tools/site.md) / [Building](../tools/building.md) / [Level](../tools/level.md) — spatial hierarchy
- [BIM Workbench](../index.md) — workbench overview
