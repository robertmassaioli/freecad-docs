# IFC Management

IFC (Industry Foundation Classes) is the open standard for BIM data exchange.
FreeCAD's BIM workbench supports both **classic IFC** (import/export) and
**Native IFC** (the document is the IFC model).

---

## IFC Import and Export

### Importing an IFC file

Use **File → Import** and select a `.ifc` file. FreeCAD loads the IFC
entities as FreeCAD objects. The import creates a hierarchy matching the
IFC spatial structure (IfcSite → IfcBuilding → IfcBuildingStorey → elements).

!!! note "Large models"
    Large IFC files (many thousands of elements) can be slow to import.
    Consider using the **IFC Explorer** (`BIM_IfcExplorer`) to inspect the
    file and select only the objects you need before a full import.

### Exporting to IFC

With a BIM model open:

1. Select all objects you want to export (or select nothing to export the
   whole document).
2. Go to **File → Export** and choose **IFC (*.ifc)**.
3. FreeCAD converts each object to its IFC representation and writes the
   file.

Before export, run **Manage → Preflight** to check for common issues
(missing IFC classes, incorrect property sets, unhosted openings).

### Native IFC mode

In Native IFC mode, the FreeCAD document is stored directly as an IFC file
(`.ifc` or `.ifcZip`) — there is no separate "export" step.

- **Create a Native IFC project:** Go to **Manage → Project** and select
  **New IFC project**.
- **Open a Native IFC file:** Open a `.ifc` file directly; FreeCAD
  automatically uses Native IFC mode.
- **Save:** Ctrl+S saves directly to the IFC file.

In Native IFC mode, all BIM objects are IFC entities. Changes are reflected
immediately in the underlying IFC data.

---

## IFC Elements {#ifc-elements}

**Menu:** Manage → Manage IFC Elements  
**Command:** `BIM_IfcElements`

Opens a spreadsheet-style panel listing all document objects with their:
- **Label** — the object's FreeCAD name
- **IFC Class** — the assigned IfcProduct subtype (e.g. `IfcWall`, `IfcColumn`)
- **IFC Type** — the predefined type (e.g. `STANDARD`, `SHEAR`)
- **Material** — assigned material name

### Changing an IFC class

Click the **IFC Class** cell and choose from the dropdown. This changes what
type the element exports as in IFC, affecting how downstream applications
interpret it.

!!! tip
    Assigning the correct IFC class is essential for interoperability —
    e.g. if a wall is exported as `IfcSlab`, structural analysis software
    will treat it as a horizontal element and apply incorrect load assumptions.

---

## IFC Quantities {#ifc-quantities}

**Menu:** Manage → Manage IFC Quantities  
**Command:** `BIM_IfcQuantities`

IFC elements carry **BaseQuantities** — standardised measurements:
- `GrossArea`, `NetArea` for slabs
- `GrossVolume`, `NetVolume` for walls
- `Length` for beams and columns

The IFC Quantities panel shows the computed quantities for all elements and
lets you verify or override them before export.

---

## IFC Properties {#ifc-properties}

**Menu:** Manage → Manage IFC Properties  
**Command:** `BIM_IfcProperties`

IFC property sets (`Pset_*`) are structured attribute groups attached to
elements. Examples:
- `Pset_WallCommon` with properties `IsExternal`, `ThermalTransmittance`, `LoadBearing`
- `Pset_ConcreteElementGeneral` with `ConstructionMethod`

The IFC Properties panel lets you:
- View which property sets are attached to each element.
- Add new property sets from the IFC schema.
- Edit individual property values.

---

## Classification {#classification}

**Menu:** Manage → Classification  
**Command:** `BIM_Classification`

Assigns a classification code (Uniclass 2015, OmniClass, ETIM, etc.) to
elements. Classification codes are stored as `IfcClassificationReference`
objects in the IFC file and are used by cost estimating and facilities
management software.

---

## Material {#material}

**Menu:** Manage → Material  
**Command:** `BIM_Material`

Assigns a material to the selected element(s). Materials can be:
- **Single material** — one named material with physical properties (density,
  thermal conductivity, colour).
- **Multi-material** — a layered assembly (e.g. wall with layers: plasterboard
  12 mm + insulation 100 mm + block 215 mm).

Materials in BIM export as `IfcMaterial` and `IfcMaterialLayerSet` entities.

### Creating a material

1. Select one or more elements.
2. Go to **Manage → Material**.
3. Either choose an existing material from the list, or create a new one.
4. For layered construction, switch to **Multi-Material** and add layers.

---

## Preflight {#preflight}

**Menu:** Manage → Preflight  
**Command:** `BIM_Preflight`

Runs a set of validation checks on the BIM model before IFC export:

| Check | What it verifies |
|-------|-----------------|
| **IFC hierarchy** | Every element is inside a valid spatial container (Site → Building → Storey) |
| **IFC classes** | No elements have the default generic `IfcBuildingElement` class |
| **Solid geometry** | All elements produce valid closed solids |
| **Openings** | All doors and windows are hosted by a wall |
| **Materials** | All structural elements have a material assigned |

Failed checks are listed with a link to the offending element. Fix all
failures before exporting for data exchange.

---

## Schedule {#schedule}

**Menu:** Manage → Schedule  
**Command:** `Arch_Schedule`

Creates a **quantity take-off table** — a spreadsheet-like document that
aggregates BIM properties across all elements. Used for:
- Material quantities (total wall area by type)
- Door and window schedules (all openings with dimensions and hardware)
- Equipment lists

### Configuring a schedule

1. Run **Manage → Schedule**.
2. Add rows, each specifying:
   - A **filter** (which IFC class, or which property set matches)
   - A **property** to aggregate (e.g. `Area`, `Volume`, `Label`)
   - An **operation** (sum, count, list)
3. The schedule populates automatically and updates when the model changes.

---

## Common mistakes and pitfalls

!!! warning "IFC export produces empty file"
    **Cause:** No spatial containment — elements are not inside a Site →
    Building → Storey hierarchy.  
    **Fix:** Run Preflight. Place all elements inside a proper hierarchy.

!!! warning "Property sets are not exported"
    **Cause:** Custom properties added directly to a FreeCAD object's
    Properties panel are **not** exported as IFC property sets — they must
    be added via the IFC Properties panel.  
    **Fix:** Use **Manage → Manage IFC Properties** to attach official
    `Pset_*` property sets.

!!! warning "Imported IFC file geometry looks incorrect"
    **Cause:** The IFC file uses representation styles not fully supported
    by FreeCAD's importer (e.g. `IfcFacetedBrep`, very large coordinates).  
    **Fix:** Try opening in Native IFC mode (open the `.ifc` directly) which
    uses the ifcopenshell library for more accurate geometry.

---

## See also

- [Structure and IFC Hierarchy](structure.md) — Site, Building, Level, Space
- [Building Elements](elements.md) — elements that carry IFC data
- [Annotation and Documentation](annotation.md) — schedules and sheets
