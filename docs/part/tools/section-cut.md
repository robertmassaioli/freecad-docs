# Persistent Section Cut

> **In one sentence:** Apply a live clipping plane to the 3-D view that
> continuously shows a cross-section of all visible objects — a permanent,
> interactive viewport cut that does not modify the actual geometry.

---

## Overview

**Workbench:** Part · **Menu:** Part → Persistent Section Cut  
**Toolbar:** Part Analysis · **Shortcut:** none

Persistent Section Cut adds an interactive clipping plane to the FreeCAD
3-D viewport. The plane can be positioned along X, Y, or Z (or at a custom
position), and the view updates in real time as you drag the cut position. The
actual Part geometry is never modified — this is a view-only operation.

---

## Intuition

Section views are essential for understanding internal geometry: seeing whether
two parts actually touch, checking wall thicknesses, and verifying clearances
without disassembling the model. Persistent Section Cut is the "X-ray mode"
of the Part workbench.

Unlike [Section](section.md) which creates a permanent wire object, Persistent
Section Cut is non-destructive: toggle it off to restore the normal view.

---

## When to use it

- Inspecting internal geometry of a complex assembly or housing.
- Verifying that two mating parts fit correctly without interference.
- Generating section-view screenshots for documentation.
- Checking wall thicknesses and internal clearances visually.

## When NOT to use it

- **Generating a section wire for further modelling** — use [Section](section.md).
- **Multiple parallel cross-sections** — use [Cross-Sections](cross-sections.md).

---

## Step-by-step walkthrough

1. **Part → Persistent Section Cut.**
2. A Section Cut panel appears.
3. Choose the **cut axis** (X / Y / Z).
4. Drag the **slider** or type a position value to move the clipping plane.
5. Toggle the **checkboxes** to cut in X, Y, or Z simultaneously (multiple
   simultaneous clip planes are supported).
6. Click the **refresh** button if the view does not update automatically.
7. **Close** the panel to restore the full view.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Cut X / Y / Z | Enable/disable clipping on each axis |
| Position X / Y / Z | Position of each clipping plane along its axis |
| Flip | Flip the clipping direction (cut the other side) |

---

## Common mistakes and pitfalls

!!! warning "Section Cut is a view tool — geometry is unchanged"
    Closing the panel or toggling off the cut restores the full view. The Part
    geometry was never modified. Do not confuse this with
    [Section](section.md) which creates a permanent geometry object.

!!! warning "Performance with many objects"
    Persistent Section Cut clips every visible object in the scene. On large
    assemblies with many high-polygon bodies, dragging the slider may be slow.
    Hide objects you don't need to inspect.

---

## Python API

The Persistent Section Cut is a GUI tool with no direct Python API for
programmatic control. For scripted cross-section generation, use
[Section](section.md) or [Cross-Sections](cross-sections.md).

```python
# For programmatic cross-sections, use Part::Section:
import FreeCAD as App
import Part

doc = App.newDocument()
shape = doc.addObject("Part::Box", "Box")
shape.Length = 30; shape.Width = 20; shape.Height = 20
doc.recompute()

# Section at Z=10
plane = Part.makePlane(60, 60, App.Vector(-15, -10, 10))
wire = shape.Shape.section(plane)
feat = doc.addObject("Part::Feature", "SectionAtZ10")
feat.Shape = wire
doc.recompute()
```

---

## See also

- [Section](section.md) — create a permanent cross-section wire object
- [Cross-Sections](cross-sections.md) — generate multiple parallel cross-section wires
- [Check Geometry](check-geometry.md) — verify geometry after inspecting internals
