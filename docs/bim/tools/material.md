# Material

> **In one sentence:** Assign a single material or a layered material
> assembly to selected BIM elements.

---

## Overview

**Workbench:** BIM  
**Menu:** Manage → Material  
**Command:** `BIM_Material`

Assigns a material to selected elements. Materials can be a single named
material or a multi-layer assembly (e.g. a composite wall with plasterboard,
insulation, and blockwork layers).

In IFC export, materials become `IfcMaterial` and `IfcMaterialLayerSet`
entities.

---

## Material types

| Type | Description |
|------|-------------|
| Single material | One named material with physical properties (density, thermal conductivity, colour) |
| Multi-material | A layered assembly — each layer has a name, material, and thickness |

---

## Step-by-step

1. Select one or more elements.
2. Go to **Manage → Material**.
3. Choose an existing material from the list, or click **New** to create one.
4. For layered construction, switch to **Multi-Material** and add layers with
   their names and thicknesses.
5. Click OK.

---

## See also

- [IFC Properties](ifc-properties.md) — `Pset_MaterialCommon` property set
- [Preflight](preflight.md) — checks that structural elements have materials
- [BIM Workbench](../index.md) — workbench overview
