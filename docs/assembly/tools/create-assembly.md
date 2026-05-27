# Assembly Setup

> **In one sentence:** Create an assembly container, insert parts into it as
> links, and optionally create new parts directly inside it — the three
> essential setup steps before adding any joints.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → New Assembly / Insert Component / New Part

| Tool | Shortcut | Description |
|------|----------|-------------|
| [New Assembly](#new-assembly) | `A` | Create a new `Assembly::AssemblyObject` container |
| [Insert Component](#insert-component) | — | Insert an existing part, body, or sub-assembly as a link |
| [New Part](#new-part) | — | Create a new empty Part Design body inside the assembly |

---

## Intuition

An assembly is a container that holds *links* to parts rather than copies of
them. This is the key architectural decision of FreeCAD's Assembly workbench:

- **Links are live** — if you modify a part in Part Design, every assembly that
  references it updates automatically.
- **Links carry their own placement** — the position and orientation of a link
  in the assembly is stored on the link, not on the original part. Moving a
  part in the assembly does not move the part in its source document.
- **Sub-assemblies work the same way** — insert an assembly into another
  assembly and you get a link to the sub-assembly, which itself contains links
  to parts.

The typical setup sequence is: New Assembly → Insert parts → Ground one part →
Add joints.

---

## New Assembly

**Menu:** Assembly → New Assembly  
**Shortcut:** `A`

Creates a new `Assembly::AssemblyObject` in the active document and activates
it. All subsequent Insert Component and joint operations apply to this assembly.

### What is created

| Object | Description |
|--------|-------------|
| `Assembly` | The root assembly container |
| `Parts` | A sub-container for inserted part links |
| `Joints` | A sub-container for joint objects |

### Multiple assemblies

A document can contain multiple assemblies (e.g. a main assembly and several
sub-assemblies). Only one assembly is *active* at a time. To switch the active
assembly, double-click it in the model tree.

### Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

doc = App.newDocument()

# Create an assembly
Gui.doCommand('import CommandCreateAssembly')
Gui.doCommand('CommandCreateAssembly.CommandCreateAssembly().Activated()')

# Or programmatically:
asm = doc.addObject("Assembly::AssemblyObject", "Assembly")
doc.recompute()
```

---

## Insert Component

**Menu:** Assembly → Insert Component → Component  
**Shortcut:** none  
**What it does:** Opens a file browser or object selector to choose an existing
part. The selected part is inserted as an App::Link into the assembly's `Parts`
container. The link is placed at the assembly origin initially; joints position
it correctly.

### Inserting from the same document

1. **Assembly → Insert Component.**
2. In the file/object dialog, select the object (Body, Part Shape, or
   sub-assembly) from the current document's tree.
3. Click **OK**.
4. The part appears at the origin. A link object (`Parts.PartName`) is created
   in the model tree.

### Inserting from an external file

1. **Assembly → Insert Component.**
2. Click **Browse** in the dialog and navigate to another `.FCStd` file.
3. Select the desired object from that file.
4. Click **OK**.
5. The external file is opened as a referenced document. The link in the
   assembly points to the object in the external file.

> **Tip:** Inserting from external files creates a permanent link between the
> documents. Moving or renaming the external file breaks the link. Use relative
> paths (keep the files in the same directory) for portability.

### Inserting the same part multiple times

Run Insert Component multiple times to insert multiple instances of the same
part (e.g. four identical bolts). Each instance is a separate link with its own
placement. Modifying the original part updates all instances simultaneously.

### Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Create a part to insert
part = doc.addObject("Part::Box", "Plate")
part.Length = 100; part.Width = 80; part.Height = 10
doc.recompute()

# Create assembly
asm = doc.addObject("Assembly::AssemblyObject", "Assembly")
parts_group = asm.newObject("App::Part", "Parts")

# Insert as a link
link = parts_group.newObject("App::Link", "Plate_Link")
link.LinkedObject = part
link.Placement = App.Placement()
doc.recompute()
```

---

## New Part

**Menu:** Assembly → Insert Component → New Part  
**What it does:** Creates a new, empty `PartDesign::Body` inside the assembly's
`Parts` container and activates it for editing. You can then model the part
in-context (seeing the other assembly parts while modelling).

### When to use it

- When you want to model a part *in the context of* the assembly — seeing the
  surrounding parts while you sketch and extrude.
- For parts that are tightly dependent on the assembled context (e.g. a bracket
  whose geometry is defined by where it connects to two adjacent parts).

### When NOT to use it

- For parts that can be designed independently — model them separately and
  insert with Insert Component. This keeps the part reusable across assemblies.

### Step-by-step

1. **Assembly → Insert Component → New Part.**
2. A new Body is created inside `Parts` and activated (highlighted in model tree).
3. Switch to the Sketcher or Part Design workbench and model the part normally.
4. The other assembly parts are visible as reference geometry while modelling.
5. When finished, double-click the Assembly to re-activate it.

---

## Common mistakes and pitfalls

!!! warning "Inserting the assembly into itself"
    FreeCAD prevents direct self-reference but take care with sub-assemblies:
    inserting sub-assembly A into B and then inserting B into A would create a
    cycle. FreeCAD may not catch this in all cases — design the assembly
    hierarchy as a tree, not a graph.

!!! warning "Editing a part after inserting breaks placement"
    If you add or remove features that change the part's bounding box or centre
    of mass significantly, the joint definitions may no longer reference the
    correct geometry. Re-check joints after significant part changes.

!!! warning "Multiple inserts of the same part from external file"
    All instances share the same external file reference. If the external file
    is updated, all instances in all assemblies referencing it will change.
    This is intentional but can cause surprises if the file is modified by
    someone else.

---

## See also

- [Grounded](grounded.md) — fix the reference part in space before adding joints
- [Joints — Kinematic](joints-kinematic.md) — constrain part motion
- [Solve Assembly](solve.md) — run the solver after adding joints
