# Export ASMT

> **In one sentence:** Export the assembly's kinematic structure — parts, joint types, and placement data — to an XML-based ASMT file for use in external multi-body dynamics solvers.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Export ASMT

Export ASMT writes the assembly structure to an **ASMT file** (Assembly Structure and Motion Transfer format) — an XML-based interchange format for kinematic assembly data. ASMT carries the structure and joint definitions; it does not embed part geometry.

---

## Intuition

FreeCAD's assembly solver handles kinematics well for visualisation and mechanism checking. For force analysis, dynamics simulation, or interoperability with external engineering software, you may need to export the assembly structure to an external solver. ASMT is the format designed for this handoff.

Think of ASMT as the "bill of joints" — analogous to the [Bill of Materials](bom.md) being the bill of parts. It describes how everything connects and moves, in a format that external tools can read.

---

## When to use it

- Feeding the assembly definition into an external multi-body dynamics (MBD) solver for force and motion analysis.
- Exchanging assembly constraint data with other CAD systems that accept ASMT input.
- Archiving the kinematic structure of an assembly in a human-readable XML format for documentation or version control.

## When NOT to use it

- **Don't use ASMT to transfer geometry** — it carries only structure and kinematics. Export part geometry separately as STEP or STL files.
- **Don't use ASMT as a general-purpose FreeCAD backup** — save the `.FCStd` file for that.

---

## What is exported

| Data | Exported? |
|------|-----------|
| Part names and placements | ✓ Yes |
| Joint types and references | ✓ Yes |
| Joint parameters (offset, angle, pitch, etc.) | ✓ Yes |
| Part geometry (B-rep, mesh) | ✗ No |
| Bill of Materials / material data | ✗ No |
| Simulation definitions | ✗ No |

---

## Step-by-step

1. Confirm the assembly is solved and complete.
2. Choose **Assembly → Export ASMT**.
3. A file save dialog opens. Navigate to the output location.
4. Enter a filename — the `.asmt` extension is added automatically if not typed.
5. Click **Save**.

The resulting file is a plain XML file that can be opened in any text editor for inspection.

---

## Parameters

Export ASMT has no task panel parameters — it writes the file immediately when you confirm the save dialog.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import FreeCADGui as Gui

# Trigger ASMT export via GUI command
Gui.doCommand('import CommandExportASMT')
Gui.doCommand('CommandExportASMT.CommandExportASMT().Activated()')
```

### Direct API call

```python
from ExportASMT import export_asmt

doc = App.ActiveDocument
asm = doc.getObject("Assembly")
export_asmt(asm, "/path/to/output.asmt")  # TODO: verify method signature
```

---

## Common mistakes and pitfalls

!!! warning "ASMT does not include geometry"
    ASMT is not a stand-alone assembly file. The receiving solver needs the
    part geometry separately — typically exported as STEP or STL files
    alongside the ASMT. ASMT only carries kinematic structure.

!!! warning "Multiple assemblies: the active one is exported"
    The export operates on the **active** assembly. Double-click the correct
    assembly in the model tree before exporting if the document contains
    multiple assemblies.

!!! warning "External solver compatibility"
    ASMT support varies between external tools. Verify that the receiving
    solver accepts FreeCAD-generated ASMT before relying on this export for
    a production workflow.

---

## See also

- [Bill of Materials](bom.md) — export the parts list (complements the ASMT structure export)
- [Solve Assembly](solve.md) — solve before exporting
- [Simulation](simulation.md) — run kinematic simulation inside FreeCAD without an external solver
