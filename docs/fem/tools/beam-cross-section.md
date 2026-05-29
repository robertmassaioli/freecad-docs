# Beam Cross Section

> **In one sentence:** Assign a cross-section shape and dimensions to edges
> modelled as 1-D beam elements, providing the area and moment of inertia
> the solver needs.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Element Geometry → Beam Cross Section

Assigns a cross-section shape and dimensions to selected edges that will be
solved as 1-D beam elements. The cross-section provides the area and second
moment of area used for bending and shear calculations.

---

## Supported cross-section types

| Type | Required dimensions |
|------|---------------------|
| Rectangular | Width, Height |
| Circular (solid) | Radius |
| Circular (hollow/tube) | Inner radius, Outer radius |
| Elliptical | Radius 1, Radius 2 |
| Box (hollow rectangle) | Width, Height, Wall thickness |
| I-section | Web height, Web thickness, Flange width, Flange thickness |
| T-section | Web height, Web thickness, Flange width, Flange thickness |
| L-section (angle) | Leg 1 length, Leg 2 length, Thickness |

---

## Step-by-step

1. Select the edge(s) to be modelled as beams.
2. Choose **FEM → Model → Element Geometry → Beam Cross Section**.
3. Select the **Section Type** and enter dimensions.
4. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Cross-section dimensions are in model units"
    All dimensions are in the document's working unit. If the document is
    in millimetres, enter `50` for a 50 mm dimension — not `0.05`.

!!! warning "Mesh must use 1-D beam elements"
    Beam cross-sections apply only to edges meshed as 1-D elements. If the
    mesh is a 3-D tetrahedral mesh, the cross-section has no effect.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

beam_sec = ObjectsFem.makeElementGeometry1D(doc, "BeamSection")
beam_sec.SectionType = "Rectangular"
beam_sec.RectWidth = 50.0    # mm
beam_sec.RectHeight = 100.0  # mm
analysis.addObject(beam_sec)
doc.recompute()
```

---

## See also

- [Beam Rotation](beam-rotation.md) — rotate the cross-section about the beam axis
- [Shell Thickness](shell-thickness.md) — thickness for 2-D shell elements
- [FEM Workbench](../index.md) — workbench overview
