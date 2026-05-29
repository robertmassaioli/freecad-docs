# Temperature Constraint

> **In one sentence:** Fix the temperature on selected faces, edges, or vertices
> to a prescribed value — the Dirichlet thermal boundary condition.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Thermal BCs → Temperature Constraint

Fixes the temperature on selected geometry to a prescribed value. This is
the thermal equivalent of the [Fixed Constraint](constraint-fixed.md) in
structural analysis — it anchors the temperature field.

---

## When to use it

- Face cooled by a heat sink held at a known temperature.
- Face in contact with a constant-temperature reservoir (isothermal wall).
- Every thermal analysis must include at least one Temperature Constraint
  to anchor the temperature scale.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Temperature | Prescribed temperature (°C or K) |
| References | Face(s), edge(s), or vertex/vertices |

---

## Common mistakes and pitfalls

!!! warning "Missing temperature constraint → singular system"
    A thermal analysis with only heat flux loads (no Temperature Constraint)
    produces an undefined temperature field. Always include at least one
    Temperature Constraint.

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

temp_bc = ObjectsFem.makeConstraintTemperature(doc, "TempBC")
temp_bc.Temperature = 20.0  # °C
temp_bc.References = [(doc.getObject("Body"), "Face1")]
analysis.addObject(temp_bc)
doc.recompute()
```

---

## See also

- [Heat Flux Load](load-heat-flux.md) — Neumann thermal BC
- [Initial Temperature](initial-temperature.md) — starting temperature for transient
- [FEM Workbench](../index.md) — workbench overview
