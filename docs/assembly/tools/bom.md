# Bill of Materials

> **In one sentence:** Generate a parts-list table directly from the assembly
> — listing every component, its quantity, description, and material — and
> optionally export the assembly definition to an ASMT file for external
> solvers.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Bill of Materials / Export ASMT

| Tool | Description |
|------|-------------|
| [Bill of Materials](#bill-of-materials) | Generate a parts list table from the assembly |
| [Export ASMT](#export-asmt) | Export assembly structure to an ASMT file |

---

## Bill of Materials

**Menu:** Assembly → Bill of Materials

### Intuition

The Bill of Materials (BOM) tool reads the assembly's parts list — every
component inserted into the `Parts` container — and creates a
`Spreadsheet::Sheet` in the document that lists:

- **Part name** — the label of each component link.
- **Quantity** — how many instances of the same underlying part appear.
- **Description** — drawn from the part's `Label2` property (the optional
  description field in the model tree).
- **Material** — drawn from the part's material properties if assigned.

The generated spreadsheet is a live FreeCAD spreadsheet object — you can
edit it, add columns, format cells, and export it to CSV using the normal
Spreadsheet workbench tools.

### When to use it

- Generating a parts list for a purchase order or manufacturing BOM.
- Creating a table to embed in a TechDraw drawing sheet alongside an
  assembly view.
- Cross-checking that every component is accounted for in the assembly.

### Step-by-step

1. Make sure the assembly is solved and complete.
2. Choose **Assembly → Bill of Materials**.
3. FreeCAD creates a new spreadsheet named `BOM` (or similar) in the
   document. The spreadsheet opens automatically.
4. Review and edit the table as needed.
5. To embed in a TechDraw drawing, insert the spreadsheet as a TechDraw
   table (TechDraw → Views → Insert Spreadsheet View).

### BOM columns

| Column | Source |
|--------|--------|
| Index | Row number |
| Name | Link label in the assembly |
| Quantity | Number of identical instances |
| Description | Part's `Label2` property |
| Material | Part's material assignment (if any) |

### Adding descriptions and materials

Set descriptions and materials on the **source parts**, not on the assembly
links. In the model tree:

- Right-click a Body or Part → **Properties**.
- Set the **Label2** field (shown as "Description" in the BOM).
- Assign a material via the Part Design or FEM material tools.

These values propagate to every assembly that references the part.

### Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

# Trigger BOM generation via GUI command
Gui.doCommand('import CommandCreateBom')
Gui.doCommand('CommandCreateBom.CommandCreateBom().Activated()')

# The result is a Spreadsheet::Sheet object named "BOM"
doc = App.ActiveDocument
bom = doc.getObject("BOM")

# Read a cell value
part_name = bom.get("B2")   # column B = name, row 2 = first data row
```

---

## Export ASMT

**Menu:** Assembly → Export ASMT

### What it does

Export ASMT writes the assembly structure — parts, links, joint definitions,
and placement data — to an **ASMT file** (Assembly Structure and Motion
Transfer format). ASMT is an XML-based format used to transfer kinematic
assembly data to external solvers and simulation environments.

### When to use it

- Feeding the assembly definition into an external multi-body dynamics (MBD)
  solver.
- Interoperability with other CAD systems that accept ASMT input.
- Archiving the assembly constraint structure in a human-readable XML format.

### What is exported

| Data | Exported? |
|------|-----------|
| Part names and placements | Yes |
| Joint types and references | Yes |
| Joint parameters (offset, angle, etc.) | Yes |
| Part geometry (B-rep, mesh) | No — geometry stays in FreeCAD |
| BOM / material data | No |

ASMT describes the *structure* and *kinematics* of the assembly. It does not
embed part geometry — the receiving solver must have its own geometry source.

### Step-by-step

1. Choose **Assembly → Export ASMT**.
2. A file save dialog opens. Choose a location and filename; the `.asmt`
   extension is added automatically.
3. Click **Save**.

The ASMT file is a plain XML file that can be opened in a text editor for
inspection.

### Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

# Export ASMT via GUI command
Gui.doCommand('import CommandExportASMT')
Gui.doCommand('CommandExportASMT.CommandExportASMT().Activated()')

# Or call the exporter directly
from ExportASMT import export_asmt
doc = App.ActiveDocument
asm = doc.getObject("Assembly")
export_asmt(asm, "/path/to/output.asmt")
```

---

## Common mistakes and pitfalls

!!! warning "BOM reflects the assembly at generation time"
    The BOM spreadsheet is a snapshot — it does not update automatically if
    you add or remove parts from the assembly later. Re-run Bill of Materials
    to regenerate an up-to-date table (this replaces the old spreadsheet).

!!! warning "Descriptions must be set on source parts, not links"
    Setting `Label2` on a link in the assembly's Parts container is not
    reflected in the BOM. Always set the description on the original body
    or part object.

!!! warning "ASMT does not embed geometry"
    ASMT is not a stand-alone assembly file. The receiving solver needs
    access to the part geometry separately (e.g. exported as STEP or STL
    files). ASMT only carries the kinematic structure.

!!! warning "Multiple assemblies in one document"
    The BOM and Export ASMT tools operate on the **active** assembly. If
    the document contains multiple assemblies, double-click the correct one
    to activate it before generating the BOM or exporting.

---

## See also

- [Assembly Setup](create-assembly.md) — insert parts into the assembly
- [Solve Assembly](solve.md) — solve the assembly before generating the BOM
- [Exploded View](exploded-view.md) — create exploded views to accompany the BOM in drawings
