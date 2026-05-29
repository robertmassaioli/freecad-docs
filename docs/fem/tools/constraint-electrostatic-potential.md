# Electrostatic Potential

> **In one sentence:** Prescribe the electric potential (voltage) on a
> boundary — the electromagnetic equivalent of a temperature constraint.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Electromagnetic BCs → Electrostatic Potential

Prescribes the electric potential (voltage, in volts) on a face, edge, or
vertex. Used with the Elmer [Electrostatic Equation](equation-electrostatic.md)
or [Magnetodynamic Equation](equation-magnetodynamic.md).

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Potential | Electric potential (V) |
| References | Face, edge, or vertex |
| Potential is constant | Fix to the same potential regardless of current |

---

## Common uses

- **Anode:** Set the positive electrode to the supply voltage (e.g. 100 V).
- **Cathode / ground:** Set the negative electrode to 0 V.
- **Floating conductor at known potential:** Set to a fixed voltage.

---

## Common mistakes and pitfalls

!!! warning "Requires the correct Elmer equation"
    Electrostatic Potential does nothing without the matching Elmer equation
    (Electrostatic or Magnetodynamic). Add the equation to the analysis.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

pot = ObjectsFem.makeConstraintElectrostaticPotential(doc, "Anode")
pot.Potential = 100.0
pot.References = [(doc.getObject("Body"), "Face2")]
analysis.addObject(pot)
doc.recompute()
```

---

## See also

- [Equation: Electrostatic](equation-electrostatic.md) — the PDE this BC drives
- [Current Density](constraint-current-density.md) — Neumann EM boundary condition
- [FEM Workbench](../index.md) — workbench overview
