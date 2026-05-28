# New Assembly

> **In one sentence:** Create the assembly container that holds all inserted parts and joints — the mandatory first step before any assembly work can begin.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → New Assembly  
**Shortcut:** `A`

New Assembly creates an `Assembly::AssemblyObject` in the active document and immediately activates it. All subsequent Insert Component operations, joint definitions, and solve operations apply to this container.

---

## Intuition

An assembly is not a collection of geometry — it is a collection of *links* to geometry that lives in Part Design bodies or Part objects. This is the key architectural decision of FreeCAD's Assembly workbench:

- **Links are live** — if you modify a part in Part Design, every assembly that references it updates automatically on the next recompute.
- **Links carry their own placement** — the position and orientation of each part in the assembly is stored on the link, not on the original part. Moving a part in the assembly does not move the original.
- **Sub-assemblies work the same way** — insert an assembly into another assembly and you get a link to the sub-assembly, which itself contains links to parts.

The typical setup sequence is: **New Assembly → Insert parts → Ground one part → Add joints → Solve.**

---

## When to use it

- Starting any new multi-part model where the parts must be positioned and constrained relative to each other.
- Creating a sub-assembly to be nested inside a larger assembly.
- When you want to simulate the motion of a mechanism rather than model it as a single solid.

## When NOT to use it

- **Don't use this to merge geometry into one solid** — for that, use Part Boolean operations or PartDesign features. Assembly does not fuse geometry.
- **Don't use this for single-part documentation** — if you just want to generate a drawing of one part, open it directly in TechDraw without an assembly container.

---

## Step-by-step

1. Choose **Assembly → New Assembly** or press `A`.
2. A new `Assembly` object appears in the model tree, containing:
   - `Parts` — an empty sub-container for inserted part links
   - `Joints` — an empty sub-container for joint objects
3. The assembly is now **active** (shown in bold in the model tree).
4. Proceed to [Insert Component](insert-component.md) to add parts.

!!! tip
    If the document already has parts in it, you can start inserting them immediately after creating the assembly — no need to save and reopen.

---

## Parameters

| Property | Type | Description |
|----------|------|-------------|
| Label | String | Name shown in the model tree (default: `Assembly`) |

The assembly container has no geometry parameters of its own; all configuration is done via the parts and joints it contains.

---

## Multiple assemblies in one document

A document can contain multiple assemblies (e.g. a main assembly and several named sub-assemblies). Only one assembly is *active* at a time. To switch the active assembly, double-click it in the model tree. All new Insert Component and joint operations apply to whichever assembly is active.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.newDocument("MyAssembly")

# Create an assembly container
asm = doc.addObject("Assembly::AssemblyObject", "Assembly")
doc.recompute()
```

### Resulting object structure

```python
# After creation the assembly contains two sub-containers:
parts_container = asm.getObject("Parts")    # holds inserted part links
joints_container = asm.getObject("Joints")  # holds joint objects
```

---

## Common mistakes and pitfalls

!!! warning "Inserting parts before creating the assembly"
    If you try to Insert Component without an active assembly, FreeCAD will
    prompt you to create one. Always create the assembly first.

!!! warning "Inserting the assembly into itself"
    FreeCAD prevents direct self-reference, but take care with sub-assemblies:
    inserting sub-assembly A into B and then B into A creates a cycle. Design
    the assembly hierarchy as a tree, not a graph.

---

## See also

- [Insert Component](insert-component.md) — add existing parts as links
- [New Part](new-part.md) — create a new part inside the assembly context
- [Toggle Grounded](grounded.md) — fix the reference part before adding joints
