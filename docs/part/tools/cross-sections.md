# Cross-Sections

> **In one sentence:** Generate multiple evenly spaced parallel cross-section
> wires through a shape at once — all cross-sections in one operation.

---

## Overview

**Workbench:** Part · **Menu:** Part → Cross-Sections  
**Toolbar:** Part Boolean · **Shortcut:** none

Cross-Sections is a dialog-based tool that creates many parallel cross-section
wires through a shape simultaneously. You choose the axis, spacing, and count.
Each cross-section is a [Section](section.md) wire — a 2-D outline at that
height.

---

## Intuition

Where [Section](section.md) computes one cross-section at a time, Cross-Sections
generates a series: imagine slicing a loaf of bread — you get all the slices at
once rather than cutting one by one. The result is a compound of wires.

Uses:
- Extracting a family of cross-sections for a loft or FEA mesh.
- Generating station lines for ship hull or aircraft analysis.
- Creating slice data for 3-D printing (slicer approximation).

---

## Step-by-step walkthrough

1. **Select the shape** to section.
2. **Part → Cross-Sections.**
3. In the dialog:
   - Select the **axis** (X, Y, or Z).
   - Set the **start** and **end** positions.
   - Set the **count** of sections (or spacing).
4. Click **OK**.
5. A compound of parallel wires appears.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Axis | XYZ enum | Direction perpendicular to the cutting planes |
| Position start | Distance | Where to start the first section |
| Position end | Distance | Where to end the last section |
| Count | Integer | Number of evenly spaced sections |

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

sphere = doc.addObject("Part::Sphere", "Sphere")
sphere.Radius = 20.0
doc.recompute()

# Generate cross-sections manually at specific Z heights
sections = []
for z in range(-15, 20, 5):   # Z = -15, -10, -5, 0, 5, 10, 15
    plane_shape = Part.makePlane(60, 60, App.Vector(-30, -30, z))
    wire = sphere.Shape.section(plane_shape)
    if not wire.isNull():
        sections.append(wire)

compound = Part.makeCompound(sections)
feat = doc.addObject("Part::Feature", "CrossSections")
feat.Shape = compound
doc.recompute()
print(f"Number of cross-sections: {len(compound.Wires)}")
```

---

## Common mistakes and pitfalls

!!! warning "Empty sections at extremes"
    If the start or end position is outside the shape's bounding box, some
    sections will be empty wires. Trim the range to the shape's extent.

---

## See also

- [Section](section.md) — single cross-section wire
- [Loft](loft.md) — use cross-sections as loft profiles
- [Persistent Section Cut](section-cut.md) — interactive viewport clipping plane
