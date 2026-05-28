# Insert Component

> **In one sentence:** Add an existing part, body, or sub-assembly into the active assembly as a live link — so changes to the original part propagate automatically.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Insert Component → Component  
**Shortcut:** none

Insert Component opens a file/object selector and places the chosen object into the assembly's `Parts` container as an `App::Link`. The link is positioned at the assembly origin initially; joints reposition it correctly after solving.

---

## Intuition

When you insert a component, FreeCAD does **not** copy the geometry into the assembly. It creates a *link* — a lightweight reference that points at the original object. This has two important consequences:

1. **Edits propagate.** If you change a pad length in Part Design, every assembly that links to that body updates automatically on recompute.
2. **Multiple instances are cheap.** Inserting the same bolt four times creates four links to one geometry object — not four copies of the geometry. A single edit updates all four at once.

---

## When to use it

- Assembling independently-modelled parts into a mechanism.
- Inserting multiple instances of the same part (e.g. four identical bolts).
- Linking parts from external `.FCStd` files to keep the assembly document separate from the part documents.

## When NOT to use it

- **Don't use this when you need to model the part in the assembly context** — for that, use [New Part](new-part.md) instead. Insert Component is for parts that already exist.
- **Don't insert geometry you intend to merge** — use Part Boolean tools for fusing shapes, not Assembly.

---

## Step-by-step

### From the same document

1. Make sure the target assembly is active (double-click it if needed).
2. Choose **Assembly → Insert Component**.
3. In the dialog, select the object (Body, Part Shape, or sub-assembly) from the current document's tree.
4. Click **OK**.
5. The part appears at the assembly origin. A link (`PartName_Link`) is created under `Parts`.

### From an external file

1. Choose **Assembly → Insert Component**.
2. Click **Browse** in the dialog and navigate to another `.FCStd` file.
3. Select the desired object from that file.
4. Click **OK**.
5. The external file is opened as a referenced document. The link points to the object in the external file.

!!! tip
    Keep all files in the same directory and use relative paths for portability. Moving or renaming the external file breaks the link.

### Inserting the same part multiple times

Run Insert Component again for each additional instance. Each instance is a separate link with its own placement. Modifying the original updates all instances simultaneously.

---

## Parameters

Insert Component has no persistent parameter panel — the dialog is a one-shot selector. After insertion, each link exposes:

| Property | Type | Description |
|----------|------|-------------|
| Label | String | Name shown in the model tree |
| Placement | Placement | Position and orientation in the assembly frame (set by joints) |
| LinkedObject | Link | Reference to the source body or part |

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.newDocument("Assembly")

# Create a source part
box = doc.addObject("Part::Box", "Plate")
box.Length = 100; box.Width = 80; box.Height = 10
doc.recompute()

# Create assembly
asm = doc.addObject("Assembly::AssemblyObject", "Assembly")
doc.recompute()

# Insert as a link (simplified — use the GUI command for the full dialog)
link = doc.addObject("App::Link", "Plate_Link")
link.LinkedObject = box
link.Placement = App.Placement()
asm.getObject("Parts").addObject(link)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "External file links break on rename"
    All instances linked to an external file share the same file reference.
    If the external file is moved or renamed, all links in all assemblies
    that reference it become invalid. Use project-relative paths and keep
    files in stable locations.

!!! warning "Editing a part after insertion may shift geometry"
    Adding or removing features that change a part's key faces may invalidate
    joint references (the face index changes). Re-check joints after
    significant part edits.

!!! warning "Inserting into the wrong assembly"
    If the document contains multiple assemblies, confirm the correct one is
    active (bold in the model tree) before inserting. Parts inserted into the
    wrong assembly must be deleted and re-inserted.

---

## See also

- [New Assembly](new-assembly.md) — create the container first
- [New Part](new-part.md) — create a new part within the assembly context
- [Toggle Grounded](grounded.md) — fix one part before adding joints
- [Joints — Kinematic](joint-fixed.md) — constrain part positions after insertion
