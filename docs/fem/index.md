# FEM Workbench

The FEM (Finite Element Method) workbench provides a complete structural,
thermal, fluid, and electromagnetic analysis workflow inside FreeCAD. It
connects the geometry of a Part Design or Part model to external open-source
solvers and then displays the results back on the original geometry.

---

## What the FEM workbench is for

FEM answers: *how does this part (or assembly) respond when forces, heat,
pressure, or electric fields are applied to it?*

Given a solid model, the FEM workflow lets you:

1. **Define materials** — assign elastic modulus, density, Poisson ratio,
   thermal conductivity, and other physical properties.
2. **Apply boundary conditions** — fix faces, apply forces, set temperatures,
   constrain displacements.
3. **Generate a mesh** — divide the geometry into small elements (tetrahedra,
   hexahedra) so the solver can process it.
4. **Run a solver** — CalculiX, Elmer, Mystran, or Z88 compute the field
   solution.
5. **Visualise results** — stress, displacement, temperature, electric
   potential, and other fields are mapped back onto the mesh and displayed
   with colour scales.

---

## Key concepts

### The analysis container

Everything in an FEM study — materials, boundary conditions, mesh, solver,
and results — lives inside an **Analysis** container (`Fem::FemAnalysis`).
You can have multiple analyses in one document (e.g. a static analysis and
a thermal analysis of the same part). Only the active analysis receives new
objects.

### Mesh

The mesh is the discrete representation of the geometry used by the solver.
FreeCAD wraps two meshing engines:

- **Netgen** — automatic tetrahedral mesher; simpler interface, built into
  the standard FreeCAD package.
- **Gmsh** — more powerful; supports structured meshes, boundary layers,
  and physical groups. Requires Gmsh to be installed separately.

Mesh quality matters: too coarse a mesh misses stress concentrations; too
fine a mesh is slow. **Mesh refinement regions** let you increase density
only where needed.

### Solvers and physics

| Solver | Physics supported |
|--------|------------------|
| **CalculiX** | Static structural, frequency (modal), buckling, thermo-mechanical, coupled/uncoupled heat transfer |
| **Elmer** | Electrostatic, electromagnetic, magnetodynamics, heat, fluid flow (Navier-Stokes), deformation |
| **Mystran** | Linear static structural (lightweight, fast) |
| **Z88** | Linear static structural |

Each solver requires its own executable to be installed; FreeCAD only
generates the input files and reads the output.

### Boundary conditions

A boundary condition is an assignment of a known value (or relationship) to
a geometric entity (face, edge, vertex):

- **Dirichlet condition** — the value is fixed (e.g. a fixed displacement = 0,
  a fixed temperature = 20 °C).
- **Neumann condition** — the flux or derivative is fixed (e.g. an applied
  force, a heat flux, a pressure).

Getting the boundary conditions right is the most important modelling step —
they define what your simulation is actually computing.

### Results and post-processing

After the solver runs, FreeCAD reads the result files and creates a
**Result** object containing field data (displacement, stress, temperature,
etc.). The simple **Show Results** tool displays a colour-mapped view with
a slider to exaggerate deformation.

For advanced post-processing, FreeCAD uses **VTK** (Visualisation Toolkit) —
the same library used by ParaView. The VTK pipeline tools allow clipping,
warping, line plots, stress linearisation, and custom calculator fields.

---

## Typical workflow

```
1. Create Analysis       ← FEM → Model → Analysis container
2. Assign materials      ← FEM → Model → Materials → Material Solid
3. Generate mesh         ← FEM → Mesh → Mesh from Shape (Netgen or Gmsh)
4. Apply BCs and loads   ← FEM → Model → Mechanical / Thermal / ...
5. Add solver            ← FEM → Solve → Solver CalculiX (or Elmer, ...)
6. Choose analysis type  ← Solver properties: static / frequency / thermomech
7. Run solver            ← FEM → Solve → Run Solver
8. View results          ← FEM → Results → Show Results
9. Post-process          ← FEM → Results → Pipeline from Result (VTK)
```

---

## Relationship to other workbenches

| Workbench | Relationship |
|-----------|-------------|
| **Part Design** | The most common source of FEM geometry — bodies are meshed directly. |
| **Part** | Part shapes and compounds can also be analysed. |
| **Spreadsheet** | Material properties and load values can be driven by spreadsheet aliases. |
| **TechDraw** | FEM result images can be inserted into TechDraw drawings as bitmap views. |

See the [Tools index](tools/index.md) for a complete reference.
