# Replace Object

> **In one sentence:** Swap one object for another throughout the document
> dependency graph, rewiring all references in a single step.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Replace Object  
**Shortcut:** none

Substitutes one `Part::Feature` with another in the document's dependency
graph. All objects that referenced the old object are updated to reference the
new one. Used when a base shape is redesigned and must replace the old version
throughout a tree of dependent features.

---

## Intuition

Think of it like a global find-and-replace in a text document, but for 3-D
objects. Every place the old object was used — as an input to a boolean, a
reference in a feature, a shape source — gets updated to point to the new
object instead. You don't have to hunt down and re-link each dependency
manually.

---

## When to use it

- You redesigned a base part and want all assemblies and dependent features
  to use the new version.
- You are swapping a placeholder shape for a final design.
- You imported an updated version of a shape and need to splice it into the
  existing document tree.

---

## When NOT to use it

- **When objects have incompatible topology** — if the old and new shapes
  have different edge/face counts, dependent features that reference specific
  topology (e.g. a Pocket on Face6) may break after replacement.

---

## Step-by-step

1. Select **2 or 3** `Part::Feature` objects:
   - 3 objects: select old, new, parent — replaces old with new inside parent.
   - 2 objects: one with no parents (old), one with a parent (new) — the
     parentless one is replaced everywhere.
2. Choose **OpenSCAD → Replace Object**.

---

## Parameters

| Input | Description |
|-------|-------------|
| Selection | 2–3 Part::Feature objects (old, new, and optionally parent) |

---

## Common mistakes and pitfalls

!!! warning "Dependent features break after replacement"
    **Cause:** The new object has different face/edge indices than the old one.
    Features that reference specific topology by index lose their references.  
    **Fix:** Check all dependent features after replacement. Re-select the
    correct face or edge references where needed.

---

## Python API

```python
import FreeCADGui as Gui
# Trigger via GUI command after selecting the objects:
Gui.doCommand("Gui.runCommand('OpenSCAD_ReplaceObject', 0)")
```

---

## See also

- [Remove Objects and Children](remove-objects-and-children.md) — clean up unused objects
- [OpenSCAD Workbench](../index.md) — workbench overview
