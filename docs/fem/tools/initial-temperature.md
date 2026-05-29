# Initial Temperature

> **In one sentence:** Set the uniform starting temperature for a transient
> thermal analysis — the temperature at t = 0.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Thermal BCs → Initial Temperature

Sets the uniform starting temperature for a **transient** thermal analysis.
At t = 0, the entire model is at this temperature; the solver then evolves
the temperature distribution over time.

---

## When to use it

- Transient heat transfer: part initially at room temperature then exposed to heat.
- Cool-down or warm-up analysis.

For **steady-state** analysis the initial temperature is irrelevant — skip it.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Initial Temperature | Starting temperature (°C or K) |

---

## Python API

```python
import ObjectsFem
doc = App.ActiveDocument
analysis = doc.getObject("Analysis")

init_temp = ObjectsFem.makeConstraintInitialTemperature(doc, "InitTemp")
init_temp.initialTemperature = 20.0  # °C
analysis.addObject(init_temp)
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Transient vs steady-state"
    Initial Temperature only affects transient analysis. In steady-state mode
    the solver ignores it. An initial temperature that does not match the
    temperature constraint values creates a thermal shock at t = 0.

---

## See also

- [Temperature Constraint](constraint-temperature.md) — fix temperature on a boundary
- [Solver CalculiX](solver-calculix.md) — set to thermomech/transient mode
- [FEM Workbench](../index.md) — workbench overview
