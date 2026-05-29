# FEM Tools

Complete index of all FEM workbench tools.

---

## Model — Setup

| Tool | Description |
|------|-------------|
| [Analysis Container](analysis-container.md) | Create an analysis container to hold all FEM objects |

---

## Model — Materials

| Tool | Description |
|------|-------------|
| [Material for Solid](material-solid.md) | Assign material properties to a solid body |
| [Material for Fluid](material-fluid.md) | Assign material properties to a fluid domain |
| [Nonlinear Mechanical Material](material-nonlinear.md) | Add nonlinear (plasticity) behaviour to a solid material |
| [Reinforced Material (Concrete)](material-reinforced.md) | Define a fibre-reinforced concrete material |
| [Material Editor](material-editor.md) | Browse and edit the FreeCAD material database |

---

## Model — Element Geometry

| Tool | Description |
|------|-------------|
| [Beam Cross Section](beam-cross-section.md) | Set the cross-section for 1-D beam elements |
| [Beam Rotation](beam-rotation.md) | Set the rotation of 1-D beam elements about their axis |
| [Shell Thickness](shell-thickness.md) | Set the thickness for 2-D shell elements |
| [Fluid Section for 1-D Flow](fluid-section.md) | Define cross-section properties for 1-D pipe-flow elements |

---

## Model — Mechanical Boundary Conditions and Loads

| Tool | Description |
|------|-------------|
| [Fixed Constraint](constraint-fixed.md) | Fix all DOF on a face, edge, or vertex |
| [Rigid Body Constraint](constraint-rigid-body.md) | Constrain a face to move as a rigid body |
| [Displacement Constraint](constraint-displacement.md) | Prescribe specific displacements and/or rotations |
| [Contact Constraint](constraint-contact.md) | Define contact (collision) between two faces |
| [Tie Constraint](constraint-tie.md) | Bond two non-touching faces together |
| [Spring Constraint](constraint-spring.md) | Add a linear spring to a face or edge |
| [Force Load](load-force.md) | Apply a total or distributed force |
| [Pressure Load](load-pressure.md) | Apply a pressure (force per area) normal to a face |
| [Centrifugal Load](load-centrifugal.md) | Apply rotational centrifugal body load |
| [Self Weight (Gravity)](load-self-weight.md) | Apply gravitational self-weight body load |

---

## Model — Thermal Boundary Conditions and Loads

| Tool | Description |
|------|-------------|
| [Initial Temperature](initial-temperature.md) | Set the starting temperature for a transient thermal analysis |
| [Heat Flux Load](load-heat-flux.md) | Apply a surface heat flux (W/m²) |
| [Temperature Constraint](constraint-temperature.md) | Fix the temperature on a face or edge |
| [Body Heat Source](load-body-heat-source.md) | Apply a volumetric heat generation rate |

---

## Model — Electromagnetic Boundary Conditions

| Tool | Description |
|------|-------------|
| [Electrostatic Potential](constraint-electrostatic-potential.md) | Prescribe electric potential on a boundary |
| [Current Density](constraint-current-density.md) | Prescribe current density on a face |
| [Magnetization](constraint-magnetization.md) | Apply magnetization on a body |
| [Electric Charge Density](constraint-electric-charge.md) | Prescribe electric charge density |

---

## Model — Fluid Boundary Conditions

| Tool | Description |
|------|-------------|
| [Initial Flow Velocity](initial-flow-velocity.md) | Set initial velocity for a fluid domain |
| [Initial Pressure](initial-pressure.md) | Set initial pressure for a fluid domain |
| [Flow Velocity Constraint](constraint-flow-velocity.md) | Prescribe flow velocity on a face |

---

## Model — Geometrical Analysis Features

| Tool | Description |
|------|-------------|
| [Plane Rotation Constraint](constraint-plane-rotation.md) | Constrain nodes to remain on a plane |
| [Section Print](section-print.md) | Output reaction forces and moments on a cross-section |
| [Transform Constraint](constraint-transform.md) | Apply loads in a local coordinate system |

---

## Mesh

| Tool | Description |
|------|-------------|
| [Mesh from Shape (Netgen)](mesh-netgen.md) | Generate a tetrahedral mesh using the built-in Netgen mesher |
| [Mesh from Shape (Gmsh)](mesh-gmsh.md) | Generate a mesh using the Gmsh mesher |
| [FEM Mesh Boundary Layer](mesh-boundary-layer.md) | Add a boundary layer refinement to a Gmsh mesh |
| [FEM Mesh Region](mesh-region.md) | Add a local mesh refinement region |
| [FEM Mesh Group](mesh-group.md) | Define a named group of mesh elements |
| [Create Elements Set](mesh-elements-set.md) | Define a set of elements for selective output or constraints |
| [Convert FEM Mesh to Mesh](mesh-convert.md) | Export the FEM mesh as a surface mesh object |

---

## Solve — Solvers

| Tool | Description |
|------|-------------|
| [Solver CalculiX](solver-calculix.md) | Add a CalculiX solver (structural, thermal, buckling, frequency) |
| [Solver Elmer](solver-elmer.md) | Add an Elmer solver (multiphysics: EM, flow, heat) |
| [Solver Mystran](solver-mystran.md) | Add a Mystran solver (linear static) |
| [Solver Z88](solver-z88.md) | Add a Z88 solver (linear static) |
| [Solver Job Control](solver-job-control.md) | Open the solver settings and output panel |
| [Run Solver](run-solver.md) | Execute the active solver |

---

## Solve — Equations (Elmer)

| Equation | Physics |
|----------|---------|
| [Deformation Equation](equation-deformation.md) | Solid deformation (large strain) |
| [Elasticity Equation](equation-elasticity.md) | Linear elasticity / stress |
| [Electrostatic Equation](equation-electrostatic.md) | Electrostatic potential and field |
| [Electricforce Equation](equation-electricforce.md) | Electric force on a body |
| [Flow Equation](equation-flow.md) | Incompressible Navier-Stokes fluid flow |
| [Flux Equation](equation-flux.md) | Heat flux or electric flux |
| [Heat Equation](equation-heat.md) | Heat conduction and convection |
| [Magnetodynamic Equation](equation-magnetodynamic.md) | Time-harmonic (AC) electromagnetic fields |
| [Magnetodynamic 2D Equation](equation-magnetodynamic-2d.md) | 2-D magnetodynamic (axisymmetric or planar) |
| [Static Current Equation](equation-static-current.md) | DC conduction current |

---

## Results

| Tool | Description |
|------|-------------|
| [Purge Results](results-purge.md) | Delete all result objects from the analysis |
| [Show Results](results-show.md) | Display a colour-mapped result field on the mesh |

---

## Results — VTK Post-Processing (requires VTK)

| Tool | Description |
|------|-------------|
| [Pipeline from Result](vtk-pipeline.md) | Create a VTK pipeline from a result object |
| [Apply Changes](vtk-apply-changes.md) | Update all post-processing filters |
| [Branch Filter](vtk-branch.md) | Split a pipeline into parallel branches |
| [Warp by Vector](vtk-warp.md) | Deform the mesh display by a vector result field |
| [Clip Scalar](vtk-clip-scalar.md) | Clip the display at a scalar threshold |
| [Cut Function](vtk-cut-function.md) | Cut the mesh with a plane, sphere, or cylinder |
| [Clip Region](vtk-clip-region.md) | Clip by a bounding box region |
| [Contours](vtk-contours.md) | Draw iso-contour lines or surfaces |
| [Glyph](vtk-glyph.md) | Draw arrows or other glyphs for vector fields |
| [Data Along Line](vtk-data-along-line.md) | Sample result data along a line and plot |
| [Linearised Stresses](vtk-linearised-stresses.md) | Compute membrane / bending / peak stress on a through-thickness line |
| [Data at Point](vtk-data-at-point.md) | Read the result value at a single point |
| [Calculator](vtk-calculator.md) | Define a custom field expression |
| [Create Filter Functions](vtk-filter-functions.md) | Define plane/sphere/cylinder functions for clip filters |

---

## Utilities

| Tool | Description |
|------|-------------|
| [Clipping Plane](clipping-plane.md) | Add or remove interactive clipping planes in the 3-D view |
| [FEM Examples](fem-examples.md) | Open a library of example FEM analyses |
