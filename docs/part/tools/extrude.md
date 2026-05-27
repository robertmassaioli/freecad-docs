# Extrude

> **In one sentence:** Push a face, wire, or edge along a direction (or along a
> custom axis) to create a solid, shell, or surface.

---

## Overview

**Workbench:** Part · **Menu:** Part → Extrude  
**Toolbar:** Part Modeling · **Shortcut:** none

Extrude takes a 2-D profile (a closed wire, face, or edge) and sweeps it
linearly along a direction, producing a 3-D solid or shell. It supports
symmetric extrusion, taper angles, multiple sub-shapes at once, and custom
extrusion directions.

---

## Intuition

Extrusion is the simplest way to turn a flat 2-D sketch into a 3-D object: draw
a rectangle, extrude it 10 mm, and you have a box. Draw an I-section, extrude
it 500 mm, and you have a steel beam.

The Part Extrude tool is more flexible than Part Design's Pad: it works on any
face or wire without requiring a Body, and it accepts custom axes, taper angles,
and solid/shell mode.

---

## When to use it

- You have a Sketcher sketch outside a Body and want a solid from it.
- You need to extrude with a taper angle (draft angle).
- You need to extrude to a symmetric depth (equal both sides).
- You want a surface (shell) rather than a solid.

## When NOT to use it

- **Inside a Part Design Body** — use Pad.
- **Non-linear sweep** — use [Sweep](sweep.md) for curved paths.

---

## Step-by-step walkthrough

1. **Select the profile** — a face, closed wire, or sketch.
2. **Part → Extrude.**
3. In the dialog:
   - Set **Direction** (default: sketch normal or Z axis).
   - Set **Length Along** (positive direction depth).
   - Optionally set **Length Against** for symmetric or asymmetric extrusion.
   - Set **Taper Angle** if you need draft.
   - Check **Create Solid** for a solid (default) or uncheck for a shell.
4. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Direction | Vector | Normal of profile | Extrusion direction |
| Length Along | Distance | 10 mm | Depth in the positive direction |
| Length Against | Distance | 0 mm | Depth in the negative direction (symmetric or asymmetric) |
| Taper Angle | Angle | 0° | Draft angle applied to the lateral faces |
| Inner Taper Angle | Angle | 0° | Draft angle for inner profiles (holes) |
| Create Solid | Bool | Yes | Solid (closed ends) vs shell (open ends) |
| Symmetric | Bool | No | Extrude equally in both directions (ignores "Against" length) |

---

## Deep dive: taper angles

A taper angle (also called draft angle in moulding) causes the extrusion to
widen or narrow as it extends. Positive taper widens the profile; negative
taper narrows it. For injection moulded parts, a typical draft is 1°–3°.

**Note:** If the taper is large enough that the profile would reduce to a point
before reaching the full length, OCCT will truncate the solid at that point.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a hexagonal wire to extrude
import math
n = 6  # hexagon
r = 10.0  # circumradius
points = []
for i in range(n):
    angle = 2 * math.pi * i / n
    points.append(App.Vector(r * math.cos(angle), r * math.sin(angle), 0))
points.append(points[0])  # close

edges = [Part.makeLine(points[i], points[i+1]) for i in range(n)]
wire = Part.Wire(edges)
face = Part.Face(wire)

# Extrude 20 mm along Z
solid = face.extrude(App.Vector(0, 0, 20))
feat = doc.addObject("Part::Feature", "HexPrism")
feat.Shape = solid
doc.recompute()

# Using the parametric Extrude object:
import PartGui  # noqa — ensures Part commands are registered
sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
# (add sketch geometry here)

ext = doc.addObject("Part::Extrusion", "Extruded")
ext.Base = sketch
ext.Dir = App.Vector(0, 0, 1)
ext.LengthFwd = 15.0
ext.LengthRev = 0.0
ext.Solid = True
ext.TaperAngle = 2.0  # degrees
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Extruding an open wire produces a shell, not a solid"
    If the profile wire is not closed, even with **Create Solid** checked, the
    result is a surface/shell. Close the wire first or use
    [Face from Wires](face-from-wires.md) to create a closed face.

!!! warning "Extrusion direction default may be unexpected"
    For a non-horizontal sketch, the default direction (profile normal) may
    not be the world Z axis. Check the Direction vector in the dialog.

---

## See also

- [Revolve](revolve.md) — rotate a profile around an axis
- [Loft](loft.md) — blend between multiple cross-sections
- [Sweep](sweep.md) — extrude along a curved path
- [Face from Wires](face-from-wires.md) — turn a wire into a face before extruding
