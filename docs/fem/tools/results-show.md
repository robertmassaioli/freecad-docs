# Show Results

> **In one sentence:** Opens a panel to display one result field on the mesh
> as a colour map, with optional deformed-shape overlay.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Show Results

Displays one computed result field (displacement, stress, temperature, etc.)
as a colour-mapped overlay on the mesh after a successful solver run. For
advanced visualisation — cutting, filtering, plotting — use the
[VTK pipeline](vtk-pipeline.md) instead.

---

## Step-by-step

1. Run the solver successfully to create a result object in the model tree.
2. Choose **FEM → Results → Show Results**.
3. The Show Results panel opens.
4. Select the **Result type** from the dropdown.
5. Optionally enable **Deformed Mesh** to visualise the deformed shape.
6. Set the **Deformation factor** — how much to exaggerate displacement
   (1.0 = true scale; typical values 10–1000× for small deformations).
7. Adjust the **Max / Min** colour scale to focus on a specific range.

---

## Result fields

| Result type | Description |
|-------------|-------------|
| Displacement | Magnitude of the displacement vector |
| Displacement x / y / z | Individual displacement components |
| Von Mises Stress | Scalar equivalent stress |
| Major Principal Stress | Maximum principal stress (tension) |
| Min Principal Stress | Minimum principal stress (compression) |
| Temperature | Nodal temperature |
| Heat flux | Magnitude of the heat flux vector |

---

## Common mistakes and pitfalls

!!! warning "Deformation factor hides real magnitudes"
    A warp factor of 1000 makes a 0.01 mm deflection look like 10 mm on
    screen. Always check the actual peak displacement value in the panel
    before reporting — the exaggerated view is for visualisation only.

!!! warning "Von Mises peaks at mesh singularities"
    Stress values at corners, re-entrant angles, and point-load locations
    are theoretically infinite. Apply loads and constraints to faces
    (distributed), not to vertices or edges, to avoid singularities.

---

## Python API

```python
import FemGui
import femresult.resulttools as resulttools

doc = App.ActiveDocument
result = doc.getObject("ResultMechanical")

# Recompute derived results (Von Mises, principal stresses)
resulttools.recomputeMechanicalMesh(result)

# Activate the analysis in the GUI
FemGui.setActiveAnalysis(doc.getObject("Analysis"))
```

---

## See also

- [Purge Results](results-purge.md) — delete result objects to free memory
- [VTK Pipeline](vtk-pipeline.md) — advanced post-processing (cut, filter, plot)
- [Run Solver](run-solver.md) — execute the solver to generate results
- [FEM Workbench](../index.md) — workbench overview
