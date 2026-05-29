# Appearance per Face

> **In one sentence:** Assign different materials, colours, or visual
> appearances to individual faces of a Part shape without modifying the
> geometry.

---

## Overview

**Workbench:** Part · **Menu:** Part → Appearance per Face  
**Toolbar:** Part Analysis · **Shortcut:** none

Appearance per Face opens a dialog that lets you select individual faces of a
shape and assign them a different material or colour. The result is a multi-
coloured or multi-material visual representation, while the geometry remains a
single Part shape.

---

## Intuition

A manufactured part often has different surface treatments: a anodised aluminium
body (grey), a rubber grip (black), and a polished stainless steel ring. You can
model all of this as a single Part solid and then assign different appearances
per face to produce an accurate visual representation without splitting the solid
into separate objects.

This is a **display-only** operation — the geometry is not modified. The face
colours are stored as view properties and appear in rendering and screenshots,
but they have no effect on mass properties, Booleans, or export geometry (STEP
exports the shape; colour data may or may not export depending on the format).

---

## When to use it

- Visualising material boundaries on a multi-material part.
- Highlighting specific faces for documentation screenshots.
- Colour-coding faces for analysis reviews (e.g., red = defect area).

## When NOT to use it

- **Actual material assignment for simulation** — use the FEM workbench's material
  assignment, which associates material properties (density, Young's modulus) with
  the volume, not just the visual face colour.
- **Splitting a solid by material** — use [Boolean Cut](boolean-cut.md) or
  [Boolean Fragments](boolean-fragments.md) and assign materials
  to separate objects.

---

## Step-by-step walkthrough

1. **Select the Part shape** in the model tree.
2. **Part → Appearance per Face.**
3. The Appearance per Face panel opens, listing all faces.
4. **Select a face** (or multiple faces with Ctrl+click) in the list or viewport.
5. Click **Set Appearance** and choose a colour or material.
6. Repeat for other faces.
7. Click **OK**.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Face selection | Select one or more faces from the shape |
| Material / Colour | Material preset or custom colour (diffuse, specular, emissive) |
| Transparency | Per-face transparency (0 = opaque, 1 = fully transparent) |

---

## Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

doc = App.newDocument()
box = doc.addObject("Part::Box", "Box")
doc.recompute()

# Set colour for specific faces via the ViewObject
view_obj = Gui.ActiveDocument.getObject("Box")

# FaceColors is a list of (r, g, b, a) tuples, one per face
# Box has 6 faces (Face1–Face6)
view_obj.DiffuseColor = [
    (0.8, 0.2, 0.2, 0.0),  # Face1 — red
    (0.2, 0.8, 0.2, 0.0),  # Face2 — green
    (0.2, 0.2, 0.8, 0.0),  # Face3 — blue
    (0.8, 0.8, 0.2, 0.0),  # Face4 — yellow
    (0.8, 0.2, 0.8, 0.0),  # Face5 — magenta
    (0.2, 0.8, 0.8, 0.0),  # Face6 — cyan
]
```

---

## Common mistakes and pitfalls

!!! warning "Per-face colours are view properties, not geometry"
    Per-face colours do not survive if the shape is copied with
    [Simple Copy](simple-copy.md) or passed through a Boolean
    operation. The new result object starts with the default colour. Re-apply
    per-face colours after any shape operation.

---

## See also

- [Check Geometry](check-geometry.md) — inspect the shape's topology
- [Defeaturing](defeaturing.md) — simplify the shape before colour-coding
