# FEM Tools

Complete index of all FEM workbench tools.

---

## Model — Setup

| Tool | Description |
|------|-------------|
| [Analysis Container](analysis.md) | Create an analysis container to hold all FEM objects |

---

## Model — Materials

| Tool | Description |
|------|-------------|
| [Material for Solid](materials.md#material-for-solid) | Assign material properties to a solid body |
| [Material for Fluid](materials.md#material-for-fluid) | Assign material properties to a fluid domain |
| [Nonlinear Mechanical Material](materials.md#nonlinear-mechanical-material) | Add nonlinear (plasticity) behaviour to a solid material |
| [Reinforced Material (Concrete)](materials.md#reinforced-material-concrete) | Define a fibre-reinforced concrete material |
| [Material Editor](materials.md#material-editor) | Browse and edit the FreeCAD material database |

---

## Model — Element Geometry

| Tool | Description |
|------|-------------|
| [Beam Cross Section](element-geometry.md#beam-cross-section) | Set the cross-section for 1-D beam elements |
| [Beam Rotation](element-geometry.md#beam-rotation) | Set the rotation of 1-D beam elements about their axis |
| [Shell Thickness](element-geometry.md#shell-thickness) | Set the thickness for 2-D shell elements |
| [Fluid Section for 1-D Flow](element-geometry.md#fluid-section-for-1-d-flow) | Define cross-section properties for 1-D pipe-flow elements |

---

## Model — Mechanical Boundary Conditions and Loads

| Tool | Description |
|------|-------------|
| [Fixed Constraint](constraints-mechanical.md#fixed-constraint) | Fix all DOF on a face, edge, or vertex |
| [Rigid Body Constraint](constraints-mechanical.md#rigid-body-constraint) | Constrain a face to move as a rigid body |
| [Displacement Constraint](constraints-mechanical.md#displacement-constraint) | Prescribe specific displacements and/or rotations |
| [Contact Constraint](constraints-mechanical.md#contact-constraint) | Define contact (collision) between two faces |
| [Tie Constraint](constraints-mechanical.md#tie-constraint) | Bond two non-touching faces together |
| [Spring Constraint](constraints-mechanical.md#spring-constraint) | Add a linear spring to a face or edge |
| [Force Load](constraints-mechanical.md#force-load) | Apply a total or distributed force |
| [Pressure Load](constraints-mechanical.md#pressure-load) | Apply a pressure (force per area) normal to a face |
| [Centrifugal Load](constraints-mechanical.md#centrifugal-load) | Apply rotational centrifugal body load |
| [Self Weight (Gravity)](constraints-mechanical.md#self-weight-gravity) | Apply gravitational self-weight body load |

---

## Model — Thermal Boundary Conditions and Loads

| Tool | Description |
|------|-------------|
| [Initial Temperature](constraints-thermal.md#initial-temperature) | Set the starting temperature for a transient thermal analysis |
| [Heat Flux Load](constraints-thermal.md#heat-flux-load) | Apply a surface heat flux (W/m²) |
| [Temperature Constraint](constraints-thermal.md#temperature-constraint) | Fix the temperature on a face or edge |
| [Body Heat Source](constraints-thermal.md#body-heat-source) | Apply a volumetric heat generation rate |

---

## Model — Electromagnetic Boundary Conditions

| Tool | Description |
|------|-------------|
| [Electrostatic Potential](constraints-other.md#electrostatic-potential) | Prescribe electric potential on a boundary |
| [Current Density](constraints-other.md#current-density) | Prescribe current density on a face |
| [Magnetization](constraints-other.md#magnetization) | Apply magnetization on a body |
| [Electric Charge Density](constraints-other.md#electric-charge-density) | Prescribe electric charge density |

---

## Model — Fluid Boundary Conditions

| Tool | Description |
|------|-------------|
| [Initial Flow Velocity](constraints-other.md#initial-flow-velocity) | Set initial velocity for a fluid domain |
| [Initial Pressure](constraints-other.md#initial-pressure) | Set initial pressure for a fluid domain |
| [Flow Velocity Constraint](constraints-other.md#flow-velocity-constraint) | Prescribe flow velocity on a face |

---

## Model — Geometrical Analysis Features

| Tool | Description |
|------|-------------|
| [Plane Rotation Constraint](constraints-other.md#plane-rotation-constraint) | Constrain nodes to remain on a plane |
| [Section Print](constraints-other.md#section-print) | Output reaction forces and moments on a cross-section |
| [Transform Constraint](constraints-other.md#transform-constraint) | Apply loads in a local coordinate system |

---

## Mesh

| Tool | Description |
|------|-------------|
| [Mesh from Shape (Netgen)](mesh.md#mesh-from-shape-netgen) | Generate a tetrahedral mesh using the built-in Netgen mesher |
| [Mesh from Shape (Gmsh)](mesh.md#mesh-from-shape-gmsh) | Generate a mesh using the Gmsh mesher |
| [FEM Mesh Boundary Layer](mesh.md#fem-mesh-boundary-layer) | Add a boundary layer refinement to a Gmsh mesh |
| [FEM Mesh Region](mesh.md#fem-mesh-region) | Add a local mesh refinement region |
| [FEM Mesh Group](mesh.md#fem-mesh-group) | Define a named group of mesh elements |
| [Create Elements Set](mesh.md#create-elements-set) | Define a set of elements for selective output or constraints |
| [Convert FEM Mesh to Mesh](mesh.md#convert-fem-mesh-to-mesh) | Export the FEM mesh as a surface mesh object |

---

## Solve — Solvers

| Tool | Description |
|------|-------------|
| [Solver CalculiX](solvers-and-equations.md#solver-calculix) | Add a CalculiX solver (structural, thermal, buckling, frequency) |
| [Solver Elmer](solvers-and-equations.md#solver-elmer) | Add an Elmer solver (multiphysics: EM, flow, heat) |
| [Solver Mystran](solvers-and-equations.md#solver-mystran) | Add a Mystran solver (linear static) |
| [Solver Z88](solvers-and-equations.md#solver-z88) | Add a Z88 solver (linear static) |
| [Solver Job Control](solvers-and-equations.md#solver-job-control) | Open the solver settings and output panel |
| [Run Solver](solvers-and-equations.md#run-solver) | Execute the active solver |

---

## Solve — Equations (Elmer)

| Equation | Physics |
|----------|---------|
| [Deformation Equation](solvers-and-equations.md#deformation-equation) | Solid deformation (large strain) |
| [Elasticity Equation](solvers-and-equations.md#elasticity-equation) | Linear elasticity / stress |
| [Electrostatic Equation](solvers-and-equations.md#electrostatic-equation) | Electrostatic potential and field |
| [Electricforce Equation](solvers-and-equations.md#electricforce-equation) | Electric force on a body |
| [Flow Equation](solvers-and-equations.md#flow-equation) | Incompressible Navier-Stokes fluid flow |
| [Flux Equation](solvers-and-equations.md#flux-equation) | Heat flux or electric flux |
| [Heat Equation](solvers-and-equations.md#heat-equation) | Heat conduction and convection |
| [Magnetodynamic Equation](solvers-and-equations.md#magnetodynamic-equation) | Time-harmonic (AC) electromagnetic fields |
| [Magnetodynamic 2D Equation](solvers-and-equations.md#magnetodynamic-2d-equation) | 2-D magnetodynamic (axisymmetric or planar) |
| [Static Current Equation](solvers-and-equations.md#static-current-equation) | DC conduction current |

---

## Results

| Tool | Description |
|------|-------------|
| [Purge Results](results.md#purge-results) | Delete all result objects from the analysis |
| [Show Results](results.md#show-results) | Display a colour-mapped result field on the mesh |

---

## Results — VTK Post-Processing (requires VTK)

| Tool | Description |
|------|-------------|
| [Apply Changes](results.md#apply-changes) | Update all post-processing filters |
| [Pipeline from Result](results.md#pipeline-from-result) | Create a VTK pipeline from a result object |
| [Branch Filter](results.md#branch-filter) | Split a pipeline into parallel branches |
| [Warp by Vector](results.md#warp-by-vector) | Deform the mesh display by a vector result field |
| [Clip Scalar](results.md#clip-scalar) | Clip the display at a scalar threshold |
| [Cut Function](results.md#cut-function) | Cut the mesh with a plane, sphere, or cylinder |
| [Clip Region](results.md#clip-region) | Clip by a bounding box region |
| [Contours](results.md#contours) | Draw iso-contour lines or surfaces |
| [Glyph](results.md#glyph) | Draw arrows or other glyphs for vector fields |
| [Data Along Line](results.md#data-along-line) | Sample result data along a line and plot |
| [Linearised Stresses](results.md#linearised-stresses) | Compute membrane / bending / peak stress on a through-thickness line |
| [Data at Point](results.md#data-at-point) | Read the result value at a single point |
| [Calculator](results.md#calculator) | Define a custom field expression |
| [Create Filter Functions](results.md#create-filter-functions) | Define plane/sphere/cylinder functions for clip filters |

---

## Utilities

| Tool | Description |
|------|-------------|
| Add Clipping Plane | Add an interactive clipping plane to the 3-D view |
| Remove All Clipping Planes | Remove all active clipping planes |
| FEM Examples | Open a library of example FEM analyses |
