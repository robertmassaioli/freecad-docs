# Bill of Materials

> **In one sentence:** Generate a live spreadsheet from the assembly listing every component, its quantity, description, and material — ready to embed in a TechDraw drawing or export to CSV.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Bill of Materials

The Bill of Materials (BOM) tool reads the active assembly's `Parts` container and creates a `Spreadsheet::Sheet` in the document that lists every component with its count and metadata.

---

## Intuition

An assembly knows exactly which parts it contains and how many instances of each. The BOM tool harvests that information into a FreeCAD spreadsheet — a live object you can format, extend, and export, or project directly onto a TechDraw drawing sheet as a parts table.

---

## When to use it

- Generating a parts list for a purchase order or manufacturing BOM.
- Creating a table to embed on a TechDraw drawing alongside an assembly view.
- Cross-checking that every component is correctly represented in the assembly.
- Exporting a CSV for import into an ERP or procurement system.

## When NOT to use it

- **Don't use BOM for a single-part drawing** — there is nothing to list. Generate the BOM only after all components are inserted and the assembly is solved.
- **Don't expect the BOM to update automatically** — it is a snapshot at generation time. Re-run it after adding or removing parts.

---

## Step-by-step

1. Confirm the assembly is solved and all components are present.
2. Choose **Assembly → Bill of Materials**.
3. FreeCAD creates a new spreadsheet named `BOM` in the document. It opens automatically.
4. Review and edit the table as needed.
5. To embed in a TechDraw drawing: **TechDraw → Views → Insert Spreadsheet View**, then select the BOM spreadsheet.
6. To export to CSV: **File → Export**, select CSV format, choose the BOM spreadsheet.

---

## BOM table structure

| Column | Source |
|--------|--------|
| Index | Row number |
| Name | Link label in the assembly |
| Quantity | Number of identical instances |
| Description | Part's `Label2` property |
| Material | Part's material assignment (if assigned) |

### Setting descriptions and materials

Set descriptions and materials on the **source parts**, not on the assembly links:

- Right-click a Body or Part in the model tree → **Properties**.
- Set the **Label2** field — this appears as the Description column in the BOM.
- Assign a material via the Part Design or FEM material tools.

These values propagate to every assembly that references the part.

---

## Parameters

The BOM tool has no task panel parameters — it generates the spreadsheet immediately when activated.

After generation, the resulting spreadsheet behaves as a standard [Spreadsheet](../../spreadsheet/index.md) workbench object and can be edited there.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import FreeCADGui as Gui

# Trigger BOM generation
Gui.doCommand('import CommandCreateBom')
Gui.doCommand('CommandCreateBom.CommandCreateBom().Activated()')

# Access the generated spreadsheet
doc = App.ActiveDocument
bom = doc.getObject("BOM")

# Read a cell
part_name = bom.get("B2")   # column B = Name, row 2 = first data row
quantity   = bom.get("C2")  # column C = Quantity
```

---

## Common mistakes and pitfalls

!!! warning "BOM is a snapshot — it does not update automatically"
    Adding or removing parts from the assembly after generating the BOM does
    not update the spreadsheet. Re-run Bill of Materials to regenerate it.
    The old spreadsheet is replaced.

!!! warning "Descriptions must be set on source parts, not on links"
    Setting `Label2` on a link inside the assembly's Parts container is not
    reflected in the BOM. Always set the description on the original Body or
    Part object.

!!! warning "Multiple assemblies: the active one is used"
    The BOM tool operates on the **active** assembly. If the document contains
    multiple assemblies, double-click the correct one before generating the BOM.

---

## See also

- [Export ASMT](export-asmt.md) — export the assembly kinematic structure to an external solver
- [Assembly Setup](new-assembly.md) — insert all parts before generating the BOM
- [Solve Assembly](solve.md) — solve before generating the BOM
- [Exploded View](exploded-view.md) — create an exploded view to accompany the BOM on a drawing
