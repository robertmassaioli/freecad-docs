# Material for Solid

> **In one sentence:** Assign elastic, thermal, and density properties to
> a solid body so the solver knows how the material responds to loads.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Materials → Material for Solid

Creates a `Fem::Material` object for a solid structural or thermal body.
This is the most commonly used material tool in FEM — every structural or
thermal analysis requires at least one.

---

## Intuition

The solver works with numbers, not names. A material object gives the solver
the numerical values it needs:

| Property | Symbol | Description |
|----------|--------|-------------|
| Young's Modulus | E | Stiffness — how hard is it to stretch? |
| Poisson's Ratio | ν | Lateral contraction ratio |
| Density | ρ | Mass per volume (needed for self-weight and frequency) |
| Thermal Conductivity | λ | Heat flow rate through material |
| Specific Heat Capacity | c | Energy stored per degree |
| Thermal Expansion Coefficient | α | Expansion per °C |

---

## Typical values (steel)

| Property | Symbol | Typical value |
|----------|--------|---------------|
| Young's Modulus | E | 210 000 MPa |
| Poisson's Ratio | ν | 0.30 |
| Density | ρ | 7 900 kg/m³ |
| Thermal Conductivity | λ | 50 W/(m·K) |
| Specific Heat Capacity | c | 500 J/(kg·K) |
| Thermal Expansion Coefficient | α | 12×10⁻⁶ /K |

---

## Step-by-step

1. Activate the Analysis container.
2. Optionally select the body to assign the material to.
3. Choose **FEM → Model → Materials → Material for Solid**.
4. In the task panel, select a material from the library or enter values.
5. Click **OK**.

---

## Common mistakes and pitfalls

!!! warning "Units must be included in property strings"
    All property values must include their unit string in FreeCAD quantity format
    (e.g. `"210000 MPa"`, not `"210000"`). The task panel handles this automatically;
    the issue arises only in Python scripts.

!!! warning "Density required for inertial loads"
    Self-weight and centrifugal loads are proportional to mass. If density is
    zero or not set, these loads produce zero force.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

mat = ObjectsFem.makeMaterial(doc, "MaterialSteel")
mat.Category = "Solid"
mat.Material = {
    "Name": "Steel",
    "YoungsModulus": "210000 MPa",
    "PoissonRatio": "0.30",
    "Density": "7900 kg/m^3",
}
analysis.addObject(mat)
doc.recompute()
```

---

## See also

- [Material for Fluid](material-fluid.md) — material for fluid domains
- [Material Editor](material-editor.md) — browse and edit the material database
- [Nonlinear Mechanical Material](material-nonlinear.md) — add plasticity
- [Analysis Container](analysis-container.md) — the study container
- [FEM Workbench](../index.md) — workbench overview
