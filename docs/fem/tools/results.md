# Results and Post-Processing

> **In one sentence:** After the solver runs, the Results tools display
> computed field data on the mesh as colour maps — and the VTK pipeline
> lets you clip, filter, plot, and derive custom quantities from those fields.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results

| Tool | Description |
|------|-------------|
| [Purge Results](#purge-results) | Delete all result objects from the analysis |
| [Show Results](#show-results) | Display a colour-mapped result field on the mesh |
| **VTK Post-Processing** | *(requires VTK support in FreeCAD build)* |
| [Pipeline from Result](#pipeline-from-result) | Create a VTK pipeline from a result object |
| [Apply Changes](#apply-changes) | Update all post-processing filters |
| [Branch Filter](#branch-filter) | Split the pipeline into parallel branches |
| [Warp by Vector](#warp-by-vector) | Deform the mesh by a vector field |
| [Clip Scalar](#clip-scalar) | Clip display at a scalar threshold |
| [Cut Function](#cut-function) | Cut the mesh with a plane or surface |
| [Clip Region](#clip-region) | Clip by a bounding box |
| [Contours](#contours) | Draw iso-contour lines or surfaces |
| [Glyph](#glyph) | Draw arrows for vector fields |
| [Data Along Line](#data-along-line) | Sample data along a line and plot |
| [Linearised Stresses](#linearised-stresses) | Membrane / bending / peak stress through thickness |
| [Data at Point](#data-at-point) | Read a single-point value |
| [Calculator](#calculator) | Define a custom field expression |
| [Create Filter Functions](#create-filter-functions) | Define plane/sphere/cylinder clip functions |

---

## Intuition

### What is a result object?

When the solver finishes, FreeCAD reads the output files and creates a
**Result** object in the analysis tree. The result object holds arrays of
field data — one value per node in the mesh — for each computed quantity:

| Field | Symbol | Units | Available in |
|-------|--------|-------|-------------|
| Displacement | u | mm | CalculiX static, Elmer elasticity |
| Von Mises stress | σ_VM | MPa | CalculiX static, Elmer elasticity |
| Principal stresses | σ₁, σ₂, σ₃ | MPa | CalculiX static |
| Temperature | T | °C / K | CalculiX thermal, Elmer heat |
| Heat flux | q | W/m² | CalculiX thermal |
| Electric potential | V | V | Elmer electrostatic |
| Velocity | v | m/s | Elmer flow |
| Pressure | p | Pa | Elmer flow |
| Magnetic potential | A | T·m | Elmer magnetodynamic |

### Simple vs VTK post-processing

**Show Results** is a simple, no-setup tool that displays one result field
at a time with a colour scale. It is sufficient for most structural analyses.

The **VTK pipeline** is for advanced work:
- Cutting the mesh at a plane to see internal values.
- Plotting stress along a line through a weld.
- Linearising stress through a vessel wall per ASME VIII.
- Animating deformation frame by frame.
- Computing derived fields (e.g. factor of safety = σ_yield / σ_VM).

---

## Purge Results

**Menu:** FEM → Results → Purge Results

Deletes all result objects (`ResultMechanical`, VTK pipelines) from the
active analysis. Use before re-running the solver if you want a clean slate,
or to free memory on a document with large result arrays.

---

## Show Results

**Menu:** FEM → Results → Show Results

Opens a panel to display one result field on the mesh as a colour map.

### Step-by-step

1. After a successful solver run, choose **FEM → Results → Show Results**.
2. The Show Results panel opens.
3. Select the **Result type** from the dropdown (displacement, stress,
   temperature, etc.).
4. Optionally enable **Deformed Mesh** to visualise the deformed shape.
5. Set the **Deformation factor** — how much to exaggerate the displacement
   (1.0 = true scale; typical values 10–1000× for small deformations).
6. Adjust the **Max / Min** of the colour scale to focus on a specific range.

### Result fields available in Show Results

| Result type | Description |
|-------------|-------------|
| Displacement | Magnitude of displacement vector |
| Displacement x/y/z | Individual displacement components |
| Von Mises Stress | Scalar equivalent stress |
| Major Principal Stress | Maximum principal stress (tension) |
| Min Principal Stress | Minimum principal stress (compression) |
| Temperature | Nodal temperature |
| Heat flux | Magnitude of heat flux vector |

### Reading peak values

The maximum and minimum values shown in the Show Results panel are the
global peak values across all nodes. The colour scale is linear between
Min and Max.

To find the stress at a specific location: use **Data at Point** in the
VTK pipeline, or read the node result from the Result object's properties.

### Python API

```python
import FemGui
import femresult.resulttools as resulttools

doc = App.ActiveDocument
result = doc.getObject("ResultMechanical")

# Compute derived results (Von Mises, principal stresses)
resulttools.recomputeMechanicalMesh(result)

# Display in GUI
FemGui.setActiveAnalysis(doc.getObject("Analysis"))
```

---

## VTK Post-Processing

The following tools are available when FreeCAD is built with VTK support.
They form a **pipeline** — each filter takes the output of the previous filter
as input, so you can chain operations.

---

### Pipeline from Result

**Menu:** FEM → Results → Pipeline from Result

Creates a VTK pipeline object from a result object. This is the starting
point for all VTK post-processing — it wraps the result data in a VTK
dataset that subsequent filters can process.

#### Step-by-step

1. Select the result object in the model tree.
2. Choose **FEM → Results → Pipeline from Result**.
3. The pipeline object appears in the tree.
4. Select the **Result field** to colour the mesh (same options as Show Results).
5. Add filters to the pipeline as needed.

---

### Apply Changes

**Menu:** FEM → Results → Apply Changes

Forces all VTK filters in the pipeline to update. Normally, changes
to filter parameters update automatically. Use **Apply Changes** if the
display appears stale after editing a filter.

---

### Branch Filter

**Menu:** FEM → Results → Branch Filter

Duplicates the pipeline at a point, creating two independent branches that
can display different fields or apply different filters to the same source
data. Useful for comparing two result quantities side by side.

---

### Warp by Vector

**Menu:** FEM → Results → Warp by Vector

Deforms the displayed mesh by adding a scaled vector field to each node
position. Used to visualise displacement: the mesh is warped by the
displacement vector, showing the deformed shape.

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Vector field | The vector result to warp by (usually Displacement) |
| Warp factor | Scale the displacement for visualisation (e.g. 100×) |

A warp factor of 1.0 shows the true deformed shape at 1:1 scale. For small
elastic deformations (< 1 mm on a 100 mm part), a factor of 100–1000 makes
the deformation visible.

---

### Clip Scalar

**Menu:** FEM → Results → Clip Scalar

Hides all elements where the selected scalar field is above (or below) a
threshold value. Useful for finding where stress exceeds a limit:

- Set the scalar field to Von Mises Stress.
- Set the threshold to the material yield stress.
- Elements above the threshold disappear, revealing only the yielded region.

---

### Cut Function

**Menu:** FEM → Results → Cut Function

Cuts the mesh with a **clip function** (plane, sphere, or cylinder) to expose
internal values. Unlike Clip Scalar (which clips by field value), Cut Function
clips by geometry.

#### Common uses

- Cross-section through the middle of a pressure vessel to see internal
  stress distribution.
- Axial cut through a rotating component to see temperature variation.
- Spherical clip to focus on a region of interest.

---

### Clip Region

**Menu:** FEM → Results → Clip Region

Clips the mesh to a rectangular bounding box region. A simpler alternative
to Cut Function when you just want to isolate one corner or zone.

---

### Contours

**Menu:** FEM → Results → Contours

Draws iso-contour surfaces (3-D) or iso-contour lines (on a cut surface)
at selected field values. Contours show where the field has a specific value
throughout the volume.

#### Parameters

| Parameter | Description |
|-----------|-------------|
| Field | Scalar field to contour |
| Number of contours | Evenly spaced between Min and Max |
| Contour values | Specific values to draw (manual entry) |

Iso-contours of temperature show the isothermal surfaces. Iso-contours of
pressure show isobaric surfaces.

---

### Glyph

**Menu:** FEM → Results → Glyph

Places a glyph (arrow, cone, or sphere) at each node, scaled and oriented
by a vector field. Used to visualise:

- Displacement direction and magnitude.
- Heat flux direction.
- Velocity vectors in a flow field.

The glyph density can be reduced (show every Nth node) for clarity in
large meshes.

---

### Data Along Line

**Menu:** FEM → Results → Data Along Line

Samples one or more result fields along a straight line through the mesh
and plots the values as an X-Y graph. Used for:

- Stress distribution through the thickness of a wall.
- Temperature gradient from a hot surface to a cold surface.
- Velocity profile across a channel.

#### Step-by-step

1. Add a Data Along Line filter to the pipeline.
2. Set the **Start Point** and **End Point** of the sampling line.
3. Set the **Resolution** (number of sample points).
4. Choose the field(s) to plot.
5. A graph appears showing field value vs position along the line.

---

### Linearised Stresses

**Menu:** FEM → Results → Linearised Stresses

Computes the **linearised stress** components through the thickness of a
wall or section, following the ASME VIII / EN 13445 pressure vessel analysis
methodology:

| Component | Description |
|-----------|-------------|
| Membrane stress | Average stress through thickness (P_m or P_L) |
| Bending stress | Linear-varying part about the mid-surface (P_b) |
| Peak stress | Non-linear residual = total − membrane − bending (F) |

This decomposition is required for pressure vessel design code checks.
Select a Data Along Line that runs through the wall thickness, then apply
Linearised Stresses to it.

---

### Data at Point

**Menu:** FEM → Results → Data at Point

Reads the result field value at a single point in the mesh, finding the
nearest node and reporting its value. Useful for:

- Checking the maximum displacement at a specific location.
- Reading the temperature at a critical point.
- Verifying against an analytical solution at a known location.

---

### Calculator

**Menu:** FEM → Results → Calculator

Defines a **custom scalar or vector field** using an expression built from
existing result fields and mathematical operations.

#### Example expressions

| Expression | Meaning |
|------------|---------|
| `Von Mises Stress / 250` | Factor of safety (yield stress = 250 MPa) |
| `Temperature - 20` | Temperature rise above ambient |
| `sqrt(Displacement_x^2 + Displacement_y^2)` | 2-D displacement magnitude |

The calculator output becomes a new field that can be displayed, contoured,
or used in further filters.

---

### Create Filter Functions

**Menu:** FEM → Results → Create Filter Functions

Defines reusable geometric functions (plane, sphere, cylinder) that can be
referenced by Cut Function or Clip Region filters. This avoids re-entering
the geometry parameters each time you use a cut.

---

## Common mistakes and pitfalls

!!! warning "VTK tools not available in standard FreeCAD package"
    The VTK post-processing pipeline requires FreeCAD to be compiled with
    VTK support. The FreeCAD AppImage and most Linux package builds include
    VTK. Windows packages may or may not. If the VTK tools are greyed out,
    check your FreeCAD build: Help → About FreeCAD → look for 'VTK' in the
    build information.

!!! warning "Deformation factor hides real magnitudes"
    A warp factor of 1000 makes a 0.01 mm deflection look like 10 mm on the
    screen. Always check the actual peak displacement value in Show Results
    before reporting results — the exaggerated view is only for visualisation.

!!! warning "Von Mises stress peaks at mesh singularities"
    Stress values at corners, re-entrant angles, and point-load application
    points are theoretically infinite (stress singularities) and will be
    very high in the FEM solution. These are artefacts of the idealised
    boundary conditions, not physically meaningful. Apply loads and
    constraints to faces (distributed), not to vertices or edges, to avoid
    singularities.

!!! warning "Linearised stresses require a through-thickness line"
    The Linearised Stresses tool only gives meaningful results when the
    Data Along Line runs from one free surface to the opposite free surface
    through the full wall thickness. A diagonal or partial line gives
    incorrect membrane and bending stress decomposition.

---

## See also

- [Solver CalculiX / Elmer](solvers-and-equations.md) — run the solver to generate results
- [Mesh](mesh.md) — mesh quality affects result accuracy
- [Mechanical Constraints and Loads](constraints-mechanical.md) — loads that drive the stress field
