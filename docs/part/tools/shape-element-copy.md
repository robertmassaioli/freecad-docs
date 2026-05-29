# Shape Element Copy

> **In one sentence:** Copy a single selected face, edge, or vertex from a
> shape into a new standalone Part feature.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Create a Copy → Shape Element Copy  
**Shortcut:** none

Extracts one topological sub-element — a single face, edge, or vertex — from
the selected shape and creates it as an independent `Part::Feature`.

---

## Intuition

Sometimes you don't need the whole solid — just one face to project onto, or
one edge to use as a sweep spine. Shape Element Copy lifts that sub-element out
of the solid so it becomes its own object that other tools can reference.

Think of it as using a scalpel to remove exactly the piece of geometry you need,
rather than working with the entire body.

---

## When to use it

- You need a specific face as a standalone surface (e.g., as a datum plane
  reference or as a target for Part → Project on Surface).
- You need a particular edge as the spine of a Part Sweep.
- You want to inspect or measure a single face in isolation.
- You need a vertex as a reference point for another operation.

## When NOT to use it

- **Don't use it to copy the whole shape** — use [Simple Copy](simple-copy.md)
  instead.
- **Don't use it for PartDesign features** — PartDesign uses sketch-based
  features with datum planes; Shape Element Copy is a Part workbench concept.

---

## Step-by-step

1. Select the parent solid in the model tree.
2. In the 3D viewport, **click** the face, edge, or vertex you want.
   (For edges and vertices, you may need to **Ctrl+click** after selecting the
   solid, or pre-select the sub-element via the 3D view before invoking the tool.)
3. Choose **Part → Create a Copy → Shape Element Copy**.
4. A new `Part::Feature` containing only that sub-element appears in the tree.

---

## Parameters

This tool has no task panel. The sub-element must be selected before invoking
the command.

---

## Common mistakes and pitfalls

!!! warning "Must have a sub-element selected"
    If only the parent solid is selected (no face/edge/vertex highlighted),
    the tool copies the entire shape rather than a sub-element. Ensure the
    correct sub-element is highlighted in the 3D view before activating the
    tool.

!!! warning "The copy is static"
    The extracted sub-element does not update if the parent solid changes.
    Re-run the tool if the parent is modified.

!!! warning "Face indices can change"
    If you later add features to the parent solid, the face that was extracted
    may acquire a different index. Always verify the sub-element is still
    correct after topology changes.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
box = doc.addObject("Part::Box", "Box")
doc.recompute()

# Extract the top face (index 5 for the top of a default Part::Box)
# Use the interactive tool for reliable sub-element selection.
# For scripting, access sub-elements directly:
top_face = box.Shape.Faces[5]
feat = doc.addObject("Part::Feature", "TopFace")
feat.Shape = top_face
doc.recompute()

print(f"Extracted: {feat.Shape.ShapeType}")   # Face
```

---

## See also

- [Simple Copy](simple-copy.md) — copy the entire shape
- [Shape Builder](shape-builder.md) — assemble new shapes from sub-elements
- [Check Geometry](check-geometry.md) — verify the extracted element
- [Part Workbench](../index.md) — workbench overview
