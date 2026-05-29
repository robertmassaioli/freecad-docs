# Shell Thickness

> **In one sentence:** Assign a uniform thickness to faces modelled as 2-D
> shell elements, determining their bending and membrane stiffness.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Element Geometry → Shell Thickness

Assigns a uniform thickness to selected faces that will be modelled as 2-D
shell elements. The shell element represents the mid-surface of the thin
feature; the thickness determines the bending stiffness and membrane stiffness.

---

## Shell vs solid elements

| | Shell elements | Solid (3-D) elements |
|-|----------------|---------------------|
| Best for | Thin-walled structures (t/L < 1/10) | Blocky or thick parts |
| Mesh | Simple mid-surface mesh | Full 3-D volume mesh |
| Typical use | Sheet metal, tanks, hulls, concrete slabs | Shafts, brackets, castings |

---

## Step-by-step

1. Select the face(s) to model as shells.
2. Choose **FEM → Model → Element Geometry → Shell Thickness**.
3. Enter the **Thickness** value.
4. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Mesh must use 2-D shell elements"
    Shell Thickness applies only to faces meshed as 2-D shell elements. If
    the mesh is a 3-D tetrahedral mesh, the thickness setting has no effect.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

shell_thick = ObjectsFem.makeElementGeometry2D(doc, "ShellThickness")
shell_thick.Thickness = 3.0  # mm
analysis.addObject(shell_thick)
doc.recompute()
```

---

## See also

- [Beam Cross Section](beam-cross-section.md) — cross-section for 1-D beam elements
- [FEM Workbench](../index.md) — workbench overview
