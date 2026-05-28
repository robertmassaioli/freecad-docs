# Element Geometry

> **In one sentence:** Element geometry objects assign cross-section or
> thickness properties to 1-D beam and 2-D shell elements — information
> that cannot be inferred from the wire or face geometry alone.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Element Geometry

| Tool | Description |
|------|-------------|
| [Beam Cross Section](#beam-cross-section) | Set the cross-section shape for 1-D beam elements |
| [Beam Rotation](#beam-rotation) | Rotate the cross-section of beam elements about the beam axis |
| [Shell Thickness](#shell-thickness) | Set the thickness of 2-D shell elements |
| [Fluid Section for 1-D Flow](#fluid-section-for-1-d-flow) | Define cross-section for 1-D pipe-flow elements |

---

## Intuition

In 3-D solid FEM, every element has geometry — the mesh captures the full
volume. But for structures that are geometrically thin in one or two
dimensions, solving a full 3-D solid mesh is wasteful:

- A **beam** is a slender rod whose cross-section dimensions are small
  compared to its length. Instead of meshing the full 3-D cross-section,
  a **1-D element** models the beam as a line, and a separate
  *cross-section object* tells the solver the area, moment of inertia, and
  shape of that cross-section.

- A **shell** is a thin flat or curved structure (sheet metal, pressure
  vessel wall, floor slab) whose thickness is small compared to its other
  dimensions. A **2-D shell element** models the mid-surface, and a separate
  *thickness object* tells the solver how thick the shell is.

The element geometry objects provide this information. Without them, beam
and shell elements cannot be solved.

---

## Beam Cross Section

**Menu:** FEM → Model → Element Geometry → Beam Cross Section

Assigns a cross-section shape and dimensions to selected edges (beam
elements). The cross-section defines the area and second moment of area
used for bending and shear calculations.

### Supported cross-section types

| Type | Required dimensions |
|------|---------------------|
| Rectangular | Width, Height |
| Circular (solid) | Radius |
| Circular (hollow/tube) | Inner radius, Outer radius |
| Elliptical | Radius 1, Radius 2 |
| Box (hollow rectangle) | Width, Height, Wall thickness |
| I-section (standard beam) | Web height, Web thickness, Flange width, Flange thickness |
| T-section | Web height, Web thickness, Flange width, Flange thickness |
| L-section (angle) | Leg 1 length, Leg 2 length, Thickness |

### Step-by-step

1. In the model tree, select the edge(s) to be modelled as beams (or leave
   unselected to apply to all beam edges in the analysis).
2. Choose **FEM → Model → Element Geometry → Beam Cross Section**.
3. In the task panel, select the **Section Type** and enter the dimensions.
4. Optionally choose the reference edges.
5. Click **OK**.

### Visualising the cross-section

FreeCAD can show a 3-D preview of the beam cross-section extruded along the
edges. Enable this in the Beam Cross Section object's **View** properties
(**Pipe Radius** and **Pipe Subdivision**).

### Python API

```python
import FreeCAD as App
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

## Beam Rotation

**Menu:** FEM → Model → Element Geometry → Beam Rotation

Rotates the cross-section of beam elements about the beam axis. By default,
the cross-section is oriented with one axis pointing upward (global Z). If
the cross-section must be rotated — for example, an I-beam with its web
horizontal rather than vertical — use Beam Rotation to set the angle.

### Step-by-step

1. Select the beam edges whose cross-section orientation needs changing.
2. Choose **FEM → Model → Element Geometry → Beam Rotation**.
3. Enter the **Rotation** angle (degrees, right-hand rule about the beam
   axis).
4. Click **OK**.

### When you need it

- I-beams where the flanges must be in the horizontal plane.
- Angle (L) sections where the leg orientation matters for bending stiffness.
- Any asymmetric cross-section (T, L, Z) where orientation affects results.

---

## Shell Thickness

**Menu:** FEM → Model → Element Geometry → Shell Thickness

Assigns a uniform thickness to selected faces that will be modelled as 2-D
shell elements. The shell element represents the mid-surface of the thin
feature; the thickness determines the bending stiffness and membrane stiffness.

### Step-by-step

1. Select the face(s) to model as shells (or leave unselected to apply to
   all shell-meshed faces in the analysis).
2. Choose **FEM → Model → Element Geometry → Shell Thickness**.
3. Enter the **Thickness** value.
4. Click **OK**.

### Shell vs solid elements

| | Shell elements | Solid (3-D) elements |
|-|---------------|---------------------|
| Best for | Thin-walled structures (t/L < 1/10) | Blocky or thick parts |
| Mesh quality | Simple mid-surface mesh | Full 3-D volume mesh |
| Result output | Membrane stress, bending stress, shear | Full 3-D stress tensor |
| Typical use | Sheet metal, tanks, hulls, slabs | Shafts, brackets, castings |

### Python API

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

## Fluid Section for 1-D Flow

**Menu:** FEM → Model → Element Geometry → Fluid Section for 1-D Flow

Defines the hydraulic properties of edges modelled as 1-D pipe-flow elements.
Used with CalculiX's network flow analysis — where fluid flows through a
network of pipes, channels, or orifices — rather than full 3-D CFD.

### Properties

| Property | Description |
|----------|-------------|
| Section Type | Pipe, Sluice, Weir, etc. |
| Inner Radius | Inner radius for circular pipe sections |
| Outer Radius | Outer radius (for annular sections) |
| Manning Number | Roughness coefficient for open-channel flow |
| Length | Characteristic length (when geometry is not explicit) |

### When to use 1-D flow

1-D network flow analysis is appropriate when:
- The flow path geometry is simple (pipes, channels, orifices).
- Detailed 3-D flow fields are not needed — only pressure drops and
  flow rates.
- The full 3-D Elmer/Navier-Stokes solver is too computationally expensive
  for the required detail level.

---

## Common mistakes and pitfalls

!!! warning "Beam cross-section dimensions are in model units"
    All cross-section dimensions (width, height, radius) are in the document's
    working unit. If the document is in millimetres, enter 50 for a 50 mm
    dimension — not 0.05. Entering in wrong units gives beam stiffness errors
    of 10³ or 10⁶ magnitude.

!!! warning "Shell thickness vs actual geometry thickness"
    The shell thickness value in the FEM object does not need to match the
    actual thickness of a 3-D body. If you are modelling a 3-D solid as an
    equivalent shell (extracting the mid-surface), set the thickness to the
    physical wall thickness of the part. If you are meshing a surface (no
    3-D solid at all), the shell thickness defines the structure.

!!! warning "Mesh must be compatible with element type"
    If you assign a Beam Cross Section but the mesh is a 3-D tetrahedral mesh,
    the beam section has no effect. Beam cross-sections apply only to edges
    that are meshed as 1-D elements. Similarly, Shell Thickness applies only
    to faces meshed as 2-D shell elements. Set the correct mesh type in the
    Gmsh mesher settings.

!!! warning "Beam orientation preview may be wrong for curved beams"
    The 3-D cross-section preview in FreeCAD is a simplified extrusion and
    may not correctly represent the orientation at curved sections. Verify
    the orientation by checking the solver input file or the result displacement.

---

## See also

- [Materials](materials.md) — material properties required alongside element geometry
- Mesh from Shape — the mesh type must match (1-D, 2-D, or 3-D elements)
- Mechanical Constraints and Loads — boundary conditions applied to beam/shell models
