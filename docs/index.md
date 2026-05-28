---
hide:
  - navigation
---

# FreeCAD Documentation

Welcome to the community-maintained FreeCAD reference documentation.
This site covers FreeCAD's workbenches in depth — written for clarity and
structured from beginner concepts through to advanced techniques and Python
scripting.

!!! info "FreeCAD version"
    This documentation targets **FreeCAD 1.1** (specifically release 1.1.1).

---

## Getting started

FreeCAD is organised around **workbenches** — collections of tools for a
specific type of task. You switch between workbenches using the workbench
selector in the toolbar. Most workflows involve at least two or three
workbenches working together.

### Common workflow paths

| Goal | Workbenches to use |
|------|--------------------|
| Design a machined or 3-D printed part | [Sketcher](sketcher/index.md) → [Part Design](part-design/index.md) → [TechDraw](techdraw/index.md) |
| Boolean solid modelling (without a Body) | [Sketcher](sketcher/index.md) → [Part](part/index.md) |
| Assemble multiple parts | [Sketcher](sketcher/index.md) → [Part Design](part-design/index.md) → [Assembly](assembly/index.md) |
| Generate CNC toolpaths | [Part Design](part-design/index.md) or [Part](part/index.md) → [CAM](cam/index.md) |
| Finite element analysis | [Part Design](part-design/index.md) → [FEM](fem/index.md) |
| Architectural / BIM model | [BIM](bim/index.md) |
| Free-form NURBS surfaces | [Sketcher](sketcher/index.md) → [Surface](surface/index.md) + [Curves Workbench](curves-wb/index.md) |
| Process a 3-D scan | [Points](points/index.md) → [Reverse Engineering](reverse-engineering/index.md) → [Mesh](mesh/index.md) |
| 2-D technical drawing | [TechDraw](techdraw/index.md) → [Spreadsheet](spreadsheet/index.md) |
| Parametric models driven by a spreadsheet | [Spreadsheet](spreadsheet/index.md) + any modelling workbench |

---

## Documented workbenches

### Core modelling

<div class="grid cards" markdown>

-   :material-pencil-ruler: **Sketcher**

    ---

    Constraint-based 2-D sketch editor. Draw geometry, apply geometric and
    dimensional constraints, and drive parametric features. Used as input by
    Part Design, Part, and many other workbenches.

    [:octicons-arrow-right-24: Sketcher](sketcher/index.md)

-   :material-cube-outline: **Part Design**

    ---

    Feature-based parametric solid modelling inside a Body. Pad, Pocket,
    Revolution, Loft, Pipe, pattern and dress-up features build fully
    parametric parts step by step.

    [:octicons-arrow-right-24: Part Design](part-design/index.md)

-   :material-vector-square: **Part**

    ---

    Direct solid and surface modelling with Boolean operations, primitives,
    extrude, revolve, sweep, loft, and surface tools. Works on bare shapes
    without a Body container.

    [:octicons-arrow-right-24: Part](part/index.md)

</div>

### Engineering and simulation

<div class="grid cards" markdown>

-   :material-link-variant: **Assembly**

    ---

    Constrain multiple parts into an assembly with kinematic joints (fixed,
    revolute, slider, ball, gear, belt…), solve positions, create exploded
    views, and run motion simulations.

    [:octicons-arrow-right-24: Assembly](assembly/index.md)

-   :material-calculator-variant: **FEM**

    ---

    Finite element analysis for mechanical, thermal, and electromagnetic
    problems. Mesh a solid, apply loads and constraints, run a solver
    (CalculiX, Elmer, Z88, OpenFOAM), and post-process results.

    [:octicons-arrow-right-24: FEM](fem/index.md)

-   :material-router-wireless: **CAM**

    ---

    Computer-aided manufacturing and G-code generation. Define a Job with
    stock and tool controllers, add 2-D, 3-D, drilling, and engraving
    operations, apply dress-ups, and post-process to machine-specific G-code.

    [:octicons-arrow-right-24: CAM](cam/index.md)

-   :material-robot-outline: **Robot**

    ---

    Industrial robot simulation. Model a 6-axis robot arm, define waypoints
    and trajectories (LIN/PTP), simulate motion, and export Kuka KRL programs.

    [:octicons-arrow-right-24: Robot](robot/index.md)

</div>

### Documentation and drawing

<div class="grid cards" markdown>

-   :material-file-document-outline: **TechDraw**

    ---

    2-D technical drawing generation. Project 3-D models to orthographic,
    isometric, or section views, add dimensions and annotations, and produce
    print-ready drawing sheets.

    [:octicons-arrow-right-24: TechDraw](techdraw/index.md)

-   :material-table: **Spreadsheet**

    ---

    Embedded spreadsheet with expression engine. Drive model dimensions from
    cells, use aliases as named parameters, and generate BOMs or part lists.

    [:octicons-arrow-right-24: Spreadsheet](spreadsheet/index.md)

-   :material-vector-line: **Draft**

    ---

    2-D and 3-D drafting, annotation, and construction geometry. Lines,
    arcs, B-splines, text, dimensions, arrays, and snapping tools — useful
    as input geometry for other workbenches.

    [:octicons-arrow-right-24: Draft](draft/index.md)

-   :material-city: **BIM**

    ---

    Building information modelling and IFC authoring. Create buildings,
    levels, walls, floors, beams, pipes, and produce IFC exports with
    full property sets and classification.

    [:octicons-arrow-right-24: BIM](bim/index.md)

</div>

### Surface and free-form modelling

<div class="grid cards" markdown>

-   :material-sine-wave: **Surface**

    ---

    Built-in NURBS surface tools. Fill a boundary of edges, extend a
    surface, create a surface from sections, or blend between edges —
    integrated with the Part kernel.

    [:octicons-arrow-right-24: Surface](surface/index.md)

-   :material-spline: **Curves Workbench** *(addon)*

    ---

    Advanced NURBS curve and surface operations. Blend curves, Gordon
    surfaces, Pipeshell sweeps, curvature combs, zebra stripe analysis,
    draft angle analysis, and more. Install via the Addon Manager.

    [:octicons-arrow-right-24: Curves Workbench](curves-wb/index.md)

</div>

### Mesh and point cloud

<div class="grid cards" markdown>

-   :material-triangle-outline: **Mesh**

    ---

    Polygon mesh operations: import/export (STL, OBJ, PLY), analyse and
    repair meshes, Boolean operations on meshes, segmentation, and
    conversion between mesh and solid.

    [:octicons-arrow-right-24: Mesh](mesh/index.md)

-   :material-dots-grid: **Points**

    ---

    Import and manage 3-D point clouds (.asc, .pcd, .ply, .e57). Supports
    unstructured and structured (grid) clouds with per-point colour,
    normal, and intensity attributes.

    [:octicons-arrow-right-24: Points](points/index.md)

-   :material-cube-scan: **Reverse Engineering**

    ---

    Convert point clouds and meshes back into parametric surfaces. Poisson
    surface reconstruction, mesh segmentation, and surface approximation
    (plane, cylinder, sphere, B-spline surface).

    [:octicons-arrow-right-24: Reverse Engineering](reverse-engineering/index.md)

</div>

### Analysis and inspection

<div class="grid cards" markdown>

-   :material-ruler: **Measure**

    ---

    Interactive measurement of distances, angles, radii, and areas directly
    in the 3-D view. Quick Measure shows properties in the status bar on
    hover; the full Measure tool stores results as model objects.

    [:octicons-arrow-right-24: Measure](measure/index.md)

-   :material-magnify-scan: **Inspection**

    ---

    Deviation analysis between an actual shape and one or more nominal
    references. Produces a false-colour heat map of signed distances for
    quality control and scan-vs-CAD comparison.

    [:octicons-arrow-right-24: Inspection](inspection/index.md)

</div>

### Other

<div class="grid cards" markdown>

-   :material-code-braces: **OpenSCAD**

    ---

    Import and export OpenSCAD `.scad` / `.csg` files, and run mesh Boolean
    operations via the OpenSCAD binary. Useful for interoperating with the
    OpenSCAD ecosystem.

    [:octicons-arrow-right-24: OpenSCAD](openscad/index.md)

</div>

---

## Which workbench should I use?

### For solid modelling: Part Design vs Part

| Situation | Use |
|-----------|-----|
| Single part with a clear feature history | **Part Design** — the feature tree keeps the design intent editable |
| Assembly of solids with Boolean ops at the top level | **Part** — no Body container required |
| Complex sweep or loft that needs B-spline profiles | **Part** (loft/sweep) or **Sketcher** + **Part Design** (additive pipe) |
| Copying or transforming existing solids | **Part** — Copy Shape, Mirror, Array tools |

### For surface modelling: Surface vs Curves Workbench vs Part

| Situation | Use |
|-----------|-----|
| Fill in a boundary of 3–4 edges | **Surface** — Fill Surface tool |
| Blend between two edges with G1/G2 continuity | **Curves Workbench** — Blend Surface |
| Surface from a network of guide curves | **Curves Workbench** — Gordon Surface |
| Revolve a profile around an axis | **Part** — Revolve |
| Unroll a cylinder/cone to flat | **Curves Workbench** — Flatten Face |

### For drawings: TechDraw vs Draft

| Situation | Use |
|-----------|-----|
| Formal engineering drawing from a 3-D model | **TechDraw** — projected views, dimensions, tolerances |
| Quick 2-D geometry for use as a guide curve | **Draft** — lines, arcs, annotations |
| BIM floor plan or section drawing | **BIM** → Section Plane → **TechDraw** page |
