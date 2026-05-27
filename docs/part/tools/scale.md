# Scale

> **In one sentence:** Scale a shape by independent X, Y, and Z factors,
> producing a uniformly or non-uniformly scaled copy.

---

## Overview

**Workbench:** Part · **Menu:** Part → Scale  
**Toolbar:** Part Modeling · **Shortcut:** none

Part Scale creates a scaled copy of the selected shape. Scaling factors can be
different for X, Y, and Z (non-uniform scaling), enabling stretching, squashing,
or mirroring (negative scale factor) along individual axes.

---

## Intuition

Scaling is rarely the primary modelling operation, but it fills several gaps:

- **Converting unit systems** — scale by 25.4 to convert inches to millimetres.
- **Proportional resizing** — scale all three axes equally to resize a complex
  imported shape.
- **Stretching a symbol** — scale only along one axis to stretch a logo or
  extrusion profile.
- **Mirroring via negative scale** — scale X by −1 to flip a shape left-to-right.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Shape | Part object | — | The shape to scale |
| Uniform | Bool | Yes | When on, a single Scale factor applies to all axes |
| Scale | Float | 1.0 | Uniform scale factor (active when Uniform = Yes) |
| Scale X | Float | 1.0 | X-axis scale factor |
| Scale Y | Float | 1.0 | Y-axis scale factor |
| Scale Z | Float | 1.0 | Z-axis scale factor |

---

## Step-by-step walkthrough

1. **Select the shape** to scale.
2. **Part → Scale.**
3. In the dialog, turn off **Uniform** if you need different factors per axis.
4. Enter the scale factors.
5. Click **OK**.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 10; box.Width = 10; box.Height = 10
doc.recompute()

# Non-uniform scale: stretch X by 2, keep Y and Z
import math
mat = App.Matrix()
mat.scale(2.0, 1.0, 1.0)   # X×2, Y×1, Z×1
scaled_shape = box.Shape.transformGeometry(mat)

feat = doc.addObject("Part::Feature", "Scaled")
feat.Shape = scaled_shape
doc.recompute()
print(f"Scaled BBox: {feat.Shape.BoundBox.XLength} × "
      f"{feat.Shape.BoundBox.YLength} × "
      f"{feat.Shape.BoundBox.ZLength}")
# → 20.0 × 10.0 × 10.0

# Parametric Scale object (FreeCAD 1.0+)
scale_obj = doc.addObject("Part::Scale", "ScaledPart")
scale_obj.Base = box
scale_obj.Uniform = False
scale_obj.XFactor = 3.0
scale_obj.YFactor = 1.0
scale_obj.ZFactor = 0.5
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Negative scale produces a mirrored shape"
    Setting Scale X = −1 mirrors the shape about the YZ plane. This is
    intentional for generating mirrored variants, but unintentional if you
    mistype a negative value.

!!! warning "Non-uniform scaling invalidates fillet/chamfer radii"
    If you scale a filleted part non-uniformly, the fillet radii are scaled
    along each axis differently, producing elliptical cross-sections. Fillet
    after scaling if uniform radii are required.

---

## See also

- [Mirror](mirror.md) — reflect a shape (special case of scale = −1)
- [Extrude](extrude.md) — linear extrusion (for resizing along one axis, extrude is more parametric)
