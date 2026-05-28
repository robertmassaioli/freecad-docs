# FEM Tools

Complete index of all FEM workbench tools.

---

## Model — Setup

| Tool | Description |
|------|-------------|
| Analysis Container | Create an analysis container to hold all FEM objects |

---

## Model — Materials

| Tool | Description |
|------|-------------|
| Material for Solid | Assign material properties to a solid body |
| Material for Fluid | Assign material properties to a fluid domain |
| Nonlinear Mechanical Material | Add nonlinear (plasticity) behaviour to a solid material |
| Reinforced Material (Concrete) | Define a fibre-reinforced concrete material |
| Material Editor | Browse and edit the FreeCAD material database |

---

## Model — Element Geometry

| Tool | Description |
|------|-------------|
| Beam Cross Section | Set the cross-section for 1-D beam elements |
| Beam Rotation | Set the rotation of 1-D beam elements about their axis |
| Shell Thickness | Set the thickness for 2-D shell elements |
| Fluid Section for 1-D Flow | Define cross-section properties for 1-D pipe-flow elements |

---

## Model — Mechanical Boundary Conditions and Loads

| Tool | Description |
|------|-------------|
| Fixed Constraint | Fix all DOF on a face, edge, or vertex |
| Rigid Body Constraint | Constrain a face to move as a rigid body |
| Displacement Constraint | Prescribe specific displacements and/or rotations |
| Contact Constraint | Define contact (collision) between two faces |
| Tie Constraint | Bond two non-touching faces together |
| Spring Constraint | Add a linear spring to a face or edge |
| Force Load | Apply a total or distributed force |
| Pressure Load | Apply a pressure (force per area) normal to a face |
| Centrifugal Load | Apply rotational centrifugal body load |
| Self Weight (Gravity) | Apply gravitational self-weight body load |

---

## Model — Thermal Boundary Conditions and Loads

| Tool | Description |
|------|-------------|
| Initial Temperature | Set the starting temperature for a transient thermal analysis |
| Heat Flux Load | Apply a surface heat flux (W/m²) |
| Temperature Constraint | Fix the temperature on a face or edge |
| Body Heat Source | Apply a volumetric heat generation rate |

---

## Model — Electromagnetic Boundary Conditions

| Tool | Description |
|------|-------------|
| Electrostatic Potential | Prescribe electric potential on a boundary |
| Current Density | Prescribe current density on a face |
| Magnetization | Apply magnetization on a body |
| Electric Charge Density | Prescribe electric charge density |

---

## Model — Fluid Boundary Conditions

| Tool | Description |
|------|-------------|
| Initial Flow Velocity | Set initial velocity for a fluid domain |
| Initial Pressure | Set initial pressure for a fluid domain |
| Flow Velocity Constraint | Prescribe flow velocity on a face |

---

## Model — Geometrical Analysis Features

| Tool | Description |
|------|-------------|
| Plane Rotation Constraint | Constrain nodes to remain on a plane |
| Section Print | Output reaction forces and moments on a cross-section |
| Transform Constraint | Apply loads in a local (cylindrical/Cartesian) coordinate system |

---

## Mesh

| Tool | Description |
|------|-------------|
| Mesh from Shape (Netgen) | Generate a tetrahedral mesh using the built-in Netgen mesher |
| Mesh from Shape (Gmsh) | Generate a mesh using the Gmsh mesher |
| FEM Mesh Boundary Layer | Add a boundary layer refinement to a Gmsh mesh |
| FEM Mesh Region | Add a local mesh refinement region |
| FEM Mesh Group | Define a named group of mesh elements |
| Create Elements Set | Define a set of elements for selective output or constraints |
| Convert FEM Mesh to Mesh | Export the FEM mesh as a surface mesh object |

---

## Solve — Solvers

| Tool | Description |
|------|-------------|
| Solver CalculiX | Add a CalculiX solver (structural, thermal, buckling, frequency) |
| Solver Elmer | Add an Elmer solver (multiphysics: EM, flow, heat) |
| Solver Mystran | Add a Mystran solver (linear static) |
| Solver Z88 | Add a Z88 solver (linear static) |
| Solver Job Control | Open the solver settings and output panel |
| Run Solver | Execute the active solver |

---

## Solve — Equations (Elmer)

| Equation | Physics |
|----------|---------|
| Deformation Equation | Solid deformation (large strain) |
| Elasticity Equation | Linear elasticity / stress |
| Electrostatic Equation | Electrostatic potential and field |
| Electricforce Equation | Electric force on a body |
| Flow Equation | Incompressible Navier-Stokes fluid flow |
| Flux Equation | Heat flux or electric flux |
| Heat Equation | Heat conduction and convection |
| Magnetodynamic Equation | Time-harmonic (AC) electromagnetic fields |
| Magnetodynamic 2D Equation | 2-D magnetodynamic (axisymmetric or planar) |
| Static Current Equation | DC conduction current |

---

## Results

| Tool | Description |
|------|-------------|
| Purge Results | Delete all result objects from the analysis |
| Show Results | Display a colour-mapped result field on the mesh |

---

## Results — VTK Post-Processing (requires VTK)

| Tool | Description |
|------|-------------|
| Apply Changes | Update all post-processing filters |
| Pipeline from Result | Create a VTK pipeline from a result object |
| Branch Filter | Split a pipeline into parallel branches |
| Warp by Vector | Deform the mesh display by a vector result field |
| Clip Scalar | Clip the display at a scalar threshold |
| Cut Function | Cut the mesh with a plane, sphere, or cylinder |
| Clip Region | Clip by a bounding box region |
| Contours | Draw iso-contour lines or surfaces |
| Glyph | Draw arrows or other glyphs for vector fields |
| Data Along Line | Sample result data along a line and plot |
| Linearised Stresses | Compute membrane / bending / peak stress on a through-thickness line |
| Data at Point | Read the result value at a single point |
| Calculator | Define a custom field expression |
| Create Filter Functions | Define plane/sphere/cylinder functions for clip filters |

---

## Utilities

| Tool | Description |
|------|-------------|
| Add Clipping Plane | Add an interactive clipping plane to the 3-D view |
| Remove All Clipping Planes | Remove all active clipping planes |
| FEM Examples | Open a library of example FEM analyses |
