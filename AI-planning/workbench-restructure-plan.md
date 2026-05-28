# Workbench Restructure Plan

**Purpose:** Actionable sprint-by-sprint guide to bring every non-compliant
workbench into full one-file-per-tool compliance as defined in
[content-architecture.md](content-architecture.md).

**Pre-reading required:** `content-architecture.md`, `tool-documentation-format.md`

---

## Scope summary

| Workbench | Merged files | Approx. tools to split out | Status |
|-----------|-------------|---------------------------|--------|
| Points | 1 | 6 | ❌ Non-compliant |
| Surface | 1 | 6 | ❌ Non-compliant |
| Reverse Engineering | 3 | 12 | ❌ Non-compliant |
| OpenSCAD | 1 | 14 | ❌ Non-compliant |
| Robot | 1 | 15 | ❌ Non-compliant |
| Part | 4 | 15 | ⚠️ Partially compliant |
| Mesh | 5 | 33 | ❌ Non-compliant |
| CAM | 4 | 35 | ❌ Non-compliant |
| BIM | 4 | 38 | ❌ Non-compliant |
| Curves WB | 3 | 37 | ❌ Non-compliant |
| FEM | 9 | ~51 | ❌ Non-compliant |
| Draft | 6 | ~64 + 16 snap modes | ❌ Non-compliant |
| TechDraw | 6 | ~77 | ❌ Non-compliant |
| **TOTAL** | **48** | **~403** | — |

Assembly was completed in the previous sprint and is ✅ fully compliant.
Part Design, Sketcher, Spreadsheet, Measure, and Inspection are ✅ already
compliant — they have individual files per tool.

---

## Sprint 0 — Quick wins (smallest workbenches)

**Target: 5 workbenches, ~53 tools total**
**Rationale:** Establish rhythm, deliver visible wins, low risk of bad decisions.

---

### 0-A: Points workbench

**Current state:** `docs/points/tools/tools.md` contains all 6 tools.

**Tools to split into individual files:**

| Tool | Target file |
|------|-------------|
| Import Points | `import-points.md` |
| Export Points | `export-points.md` |
| Convert to Points | `convert-to-points.md` |
| Structured Point Cloud | `structured-point-cloud.md` |
| Cut Point Cloud | `cut-point-cloud.md` |
| Merge Point Clouds | `merge-point-clouds.md` |

**Nav entries to add** (replace the single `tools/tools.md` entry):
```yaml
- Points:
  - points/index.md
  - Tools:
    - points/tools/index.md
    - Import Points: points/tools/import-points.md
    - Export Points: points/tools/export-points.md
    - Convert to Points: points/tools/convert-to-points.md
    - Structured Point Cloud: points/tools/structured-point-cloud.md
    - Cut Point Cloud: points/tools/cut-point-cloud.md
    - Merge Point Clouds: points/tools/merge-point-clouds.md
```

**Files to delete:** `docs/points/tools/tools.md`

---

### 0-B: Surface workbench

**Current state:** `docs/surface/tools/tools.md` contains 6 tools. Note that
`Filling` is listed as a category header in the file — the actual tool is
named **Fill** (officially "Part Design Fill" equivalent for surface patches).
Verify the exact FreeCAD menu names before writing.

**Tools to split into individual files:**

| Tool | Target file |
|------|-------------|
| Filling | `filling.md` |
| Fill Boundary Curves | `fill-boundary-curves.md` |
| Sections | `sections.md` |
| Extend Face | `extend-face.md` |
| Blend Curve | `blend-curve.md` |
| Curve on Mesh | `curve-on-mesh.md` |

**Files to delete:** `docs/surface/tools/tools.md`

---

### 0-C: Reverse Engineering workbench

**Current state:** 3 merged files.

#### Approximation tools (split `approximation.md` → 6 files)

| Tool | Target file |
|------|-------------|
| Plane | `approx-plane.md` |
| Cylinder | `approx-cylinder.md` |
| Sphere | `approx-sphere.md` |
| Polynomial Surface | `approx-polynomial-surface.md` |
| Approximate B-Spline Surface | `approx-bspline-surface.md` |
| Approximate B-Spline Curve | `approx-bspline-curve.md` |

#### Segmentation tools (split `segmentation.md` → 4 files)

| Tool | Target file |
|------|-------------|
| Mesh Segmentation | `mesh-segmentation.md` |
| Manual Segmentation | `manual-segmentation.md` |
| From Components | `from-components.md` |
| Wire From Mesh Boundary | `wire-from-mesh-boundary.md` |

#### Surface reconstruction tools (split `surface-reconstruction.md` → 2 files)

| Tool | Target file |
|------|-------------|
| Poisson Reconstruction | `poisson-reconstruction.md` |
| Structured Point Clouds | `structured-point-clouds.md` |

**Files to delete:** `approximation.md`, `segmentation.md`, `surface-reconstruction.md`

---

### 0-D: OpenSCAD workbench

**Current state:** `docs/openscad/tools/tools.md` contains 14 tools.

**Tools to split into individual files:**

| Tool | Target file |
|------|-------------|
| Replace Object | `replace-object.md` |
| Remove Objects and Children | `remove-objects-and-children.md` |
| Refine Shape Feature | `refine-shape-feature.md` |
| Mirror Mesh Feature | `mirror-mesh-feature.md` |
| Scale Mesh Feature | `scale-mesh-feature.md` |
| Resize Mesh Feature | `resize-mesh-feature.md` |
| Increase Tolerance Feature | `increase-tolerance-feature.md` |
| Convert Edges to Faces | `convert-edges-to-faces.md` |
| Expand Placements | `expand-placements.md` |
| Explode Group | `explode-group.md` |
| Add OpenSCAD Element | `add-openscad-element.md` |
| Mesh Boolean | `mesh-boolean.md` |
| Hull | `hull.md` |
| Minkowski Sum | `minkowski-sum.md` |

**Files to delete:** `docs/openscad/tools/tools.md`

---

### 0-E: Robot workbench

**Current state:** `docs/robot/tools/tools.md` contains 15 tools.
⚠️ The Robot workbench is deprecated in FreeCAD 1.0. Note this in each tool's
**Overview** section ("This tool belongs to the deprecated Robot workbench...")
but still document it fully — the docs are a historical record.

**Tools to split into individual files:**

| Tool | Target file |
|------|-------------|
| Tool (robot tool definition) | `robot-tool.md` |
| Place Robot | `place-robot.md` |
| Set Home Position | `set-home-position.md` |
| Move to Home | `move-to-home.md` |
| Trajectory | `trajectory.md` |
| Insert in Trajectory (tool position) | `insert-trajectory-tool-position.md` |
| Insert in Trajectory (preselect position) | `insert-trajectory-preselect.md` |
| Set Default Orientation | `set-default-orientation.md` |
| Set Default Values | `set-default-values.md` |
| Edge to Trajectory | `edge-to-trajectory.md` |
| Dress-Up Trajectory | `dress-up-trajectory.md` |
| Trajectory Compound | `trajectory-compound.md` |
| Simulate Trajectory | `simulate-trajectory.md` |
| Kuka Compact Subroutine | `kuka-compact-subroutine.md` |
| Kuka Full Subroutine | `kuka-full-subroutine.md` |

**Files to delete:** `docs/robot/tools/tools.md`

---

## Sprint 1 — Part workbench cleanup

**Target: 4 merged files, 15 tools**
**Rationale:** Part is already mostly compliant (most tools have individual files).
This sprint closes the remaining gaps without risk of disrupting links into the
many existing compliant Part tool pages.

**Note:** `primitives.md` covers all primitive shapes as a group, but individual
primitive files (`cube.md`, `cylinder.md`, `sphere.md`, `cone.md`, `torus.md`,
`tube.md`) already exist. `primitives.md` should be **converted to a concept page**
(overview of Part primitives as a category), not split further — it already
delegates to individual pages.

### 1-A: copy-tools.md → 4 files

| Tool | Target file |
|------|-------------|
| Simple Copy | `simple-copy.md` |
| Transformed Copy | `transformed-copy.md` |
| Shape Element Copy | `shape-element-copy.md` |
| Refine Shape | `refine-shape.md` |

### 1-B: join-features.md → 3 files

| Tool | Target file |
|------|-------------|
| Connect Shapes | `connect-shapes.md` |
| Embed Shapes | `embed-shapes.md` |
| Cutout Shape | `cutout-shape.md` |

### 1-C: split-features.md → 4 files

| Tool | Target file |
|------|-------------|
| Boolean Fragments | `boolean-fragments.md` |
| Slice Apart | `slice-apart.md` |
| Slice to Compound | `slice-to-compound.md` |
| Boolean XOR | `boolean-xor.md` |

### 1-D: mesh-and-solid.md → 4 files

| Tool | Target file |
|------|-------------|
| Shape from Mesh | `shape-from-mesh.md` |
| Points from Shape | `points-from-shape.md` |
| Convert to Solid | `convert-to-solid.md` |
| Reverse Shapes | `reverse-shapes.md` |

**Files to delete:** `copy-tools.md`, `join-features.md`, `split-features.md`,
`mesh-and-solid.md`

---

## Sprint 2 — Mesh workbench

**Target: 5 merged files, 33 tools**
**Rationale:** Clean tool taxonomy, each category maps cleanly to individual
commands, no special design decisions needed.

### Import/Export (split `import-export.md` → 5 files)

| Tool | Target file |
|------|-------------|
| Import Mesh | `import-mesh.md` |
| Export Mesh | `export-mesh.md` |
| From Part Shape | `from-part-shape.md` |
| Remesh with Gmsh | `remesh-gmsh.md` |
| Build Regular Solid | `build-regular-solid.md` |

### Analyse (split `analyse.md` → 6 files)

| Tool | Target file |
|------|-------------|
| Evaluate and Repair | `evaluate-repair.md` |
| Evaluate Facet | `evaluate-facet.md` |
| Curvature Info | `curvature-info.md` |
| Evaluate Solid | `evaluate-solid.md` |
| Bounding Box | `bounding-box.md` |
| Vertex Curvature | `vertex-curvature.md` |

### Modify (split `modify.md` → 10 files)

| Tool | Target file |
|------|-------------|
| Harmonize Normals | `harmonize-normals.md` |
| Flip Normals | `flip-normals.md` |
| Fill Up Holes | `fill-up-holes.md` |
| Fill Hole Interactively | `fill-hole-interactively.md` |
| Add Facet | `add-facet.md` |
| Remove Components | `remove-components.md` |
| Remove Component by Hand | `remove-component-by-hand.md` |
| Smoothing | `smoothing.md` |
| Decimating | `decimating.md` |
| Scale | `scale.md` |

### Boolean/Cutting (split `boolean-cutting.md` → 8 files)

| Tool | Target file |
|------|-------------|
| Union | `union.md` |
| Intersection | `intersection.md` |
| Difference | `difference.md` |
| Cut by Polygon | `cut-by-polygon.md` |
| Trim by Polygon | `trim-by-polygon.md` |
| Trim by Plane | `trim-by-plane.md` |
| Section by Plane | `section-by-plane.md` |
| Cross-Sections | `cross-sections.md` |

### Segmentation (split `segmentation.md` → 4 files)

| Tool | Target file |
|------|-------------|
| Merge | `merge.md` |
| Split Components | `split-components.md` |
| Segmentation (Curvature) | `segmentation-curvature.md` |
| Segmentation — Best Fit | `segmentation-best-fit.md` |

**Files to delete:** `import-export.md`, `analyse.md`, `modify.md`,
`boolean-cutting.md`, `segmentation.md`

---

## Sprint 3 — CAM workbench

**Target: 4 merged files, 35 tools + 2 concept pages**
**Rationale:** Tools are well-named, map directly to FreeCAD menu entries.
Two concept pages (`common-operation-parameters.md` and `feeds-and-speeds.md`)
are legitimately shared-concept material, not tool pages.

### Job tools (split `job.md` → 6 files)

| Tool | Target file |
|------|-------------|
| New Job | `new-job.md` |
| Post Process | `post-process.md` |
| Sanity Check | `sanity-check.md` |
| Inspect Path | `inspect-path.md` |
| Simulate | `simulate.md` |
| Toggle Active | `toggle-active.md` |

### Operations (split `operations.md` → 17 files + 1 concept page)

The "Common parameters" section in `operations.md` is **conceptual content** shared
by all operations — keep it as `docs/cam/concepts/operation-parameters.md`,
not as a tool page. Every individual operation page should link to it.

| Tool | Target file |
|------|-------------|
| Profile | `op-profile.md` |
| Pocket Shape | `op-pocket-shape.md` |
| Face Milling | `op-face-milling.md` |
| Helix | `op-helix.md` |
| Adaptive Clearing | `op-adaptive-clearing.md` |
| Slot | `op-slot.md` |
| 3D Pocket | `op-3d-pocket.md` |
| Surface | `op-surface.md` |
| Waterline | `op-waterline.md` |
| Drilling | `op-drilling.md` |
| Engrave | `op-engrave.md` |
| Deburr | `op-deburr.md` |
| V-Carve | `op-v-carve.md` |
| Comment | `op-comment.md` |
| Stop | `op-stop.md` |
| Custom | `op-custom.md` |
| Probe | `op-probe.md` |

**Concept page:** `docs/cam/concepts/operation-parameters.md`

### Dress-ups (split `dressup.md` → 9 files)

| Tool | Target file |
|------|-------------|
| Dogbone | `dressup-dogbone.md` |
| Holding Tabs | `dressup-holding-tabs.md` |
| Lead In/Out | `dressup-lead-in-out.md` |
| Ramp Entry | `dressup-ramp-entry.md` |
| Boundary | `dressup-boundary.md` |
| Axis Map | `dressup-axis-map.md` |
| Z Correct | `dressup-z-correct.md` |
| Array (dress-up) | `dressup-array.md` |
| Drag Knife | `dressup-drag-knife.md` |

### Tool bits (split `toolbits.md` → 3 files + 2 concept pages)

The "Tool bit files" section and "Calculating feeds and speeds" section are
**concept pages** — they document how the system works, not individual commands.

| Tool | Target file |
|------|-------------|
| Tool Bit Library | `toolbit-library.md` |
| Tool Bit Dock | `toolbit-dock.md` |
| Tool Controller | `tool-controller.md` |

**Concept pages:**
- `docs/cam/concepts/tool-bit-files.md`
- `docs/cam/concepts/feeds-and-speeds.md`

**Files to delete:** `job.md`, `operations.md`, `dressup.md`, `toolbits.md`

---

## Sprint 4 — BIM workbench

**Target: 4 merged files, 38 tools**

### Structure (split `structure.md` → 4 files)

| Tool | Target file |
|------|-------------|
| Site | `site.md` |
| Building | `building.md` |
| Level | `level.md` |
| Space | `space.md` |

### Elements (split `elements.md` → 17 files)

| Tool | Target file |
|------|-------------|
| Wall | `wall.md` |
| Curtain Wall | `curtain-wall.md` |
| Column | `column.md` |
| Beam | `beam.md` |
| Slab | `slab.md` |
| Door | `door.md` |
| Window | `window.md` |
| Stairs | `stairs.md` |
| Roof | `roof.md` |
| Panel | `panel.md` |
| Frame | `frame.md` |
| Fence | `fence.md` |
| Truss | `truss.md` |
| Equipment | `equipment.md` |
| Rebar | `rebar.md` |
| Pipe | `pipe.md` |
| Pipe Connector | `pipe-connector.md` |

### Annotation (split `annotation.md` → 9 files)

| Tool | Target file |
|------|-------------|
| Text | `text.md` |
| Dimensions | `dimensions.md` |
| Leader | `leader.md` |
| Axis | `axis.md` |
| Axis System | `axis-system.md` |
| Grid | `grid.md` |
| Section Plane | `section-plane.md` |
| Drawing Page | `drawing-page.md` |
| Drawing View | `drawing-view.md` |

### IFC (split `ifc.md` → 7 files + 1 concept page)

"IFC Import and Export" is a workflow overview (how IFC import/export works in
general) — treat as a concept page. The rest are individual tool commands.

| Tool | Target file |
|------|-------------|
| IFC Elements | `ifc-elements.md` |
| IFC Quantities | `ifc-quantities.md` |
| IFC Properties | `ifc-properties.md` |
| Classification | `ifc-classification.md` |
| Material | `ifc-material.md` |
| Preflight | `ifc-preflight.md` |
| Schedule | `ifc-schedule.md` |

**Concept page:** `docs/bim/concepts/ifc-import-export.md`

**Files to delete:** `structure.md`, `elements.md`, `annotation.md`, `ifc.md`

---

## Sprint 5 — FEM workbench

**Target: 9 merged files, ~51 tools + concept pages**
**Special consideration: Elmer equations subsystem**

### Analysis management (split `analysis.md` → 2 files)

| Tool | Target file |
|------|-------------|
| New Analysis | `new-analysis.md` |
| Activate Analysis | `activate-analysis.md` |

The "Multiple analyses" workflow section becomes a concept page:
`docs/fem/concepts/multiple-analyses.md`

### Materials (split `materials.md` → 5 files)

| Tool | Target file |
|------|-------------|
| Material for Solid | `material-for-solid.md` |
| Material for Fluid | `material-for-fluid.md` |
| Nonlinear Mechanical Material | `material-nonlinear.md` |
| Reinforced Material (Concrete) | `material-reinforced.md` |
| Material Editor | `material-editor.md` |

### Element Geometry (split `element-geometry.md` → 4 files)

| Tool | Target file |
|------|-------------|
| Beam Cross Section | `beam-cross-section.md` |
| Beam Rotation | `beam-rotation.md` |
| Shell Thickness | `shell-thickness.md` |
| Fluid Section for 1-D Flow | `fluid-section-1d.md` |

### Mechanical Constraints (split `constraints-mechanical.md` → 10 files)

| Tool | Target file |
|------|-------------|
| Fixed Constraint | `constraint-fixed.md` |
| Rigid Body Constraint | `constraint-rigid-body.md` |
| Displacement Constraint | `constraint-displacement.md` |
| Contact Constraint | `constraint-contact.md` |
| Tie Constraint | `constraint-tie.md` |
| Spring Constraint | `constraint-spring.md` |
| Force Load | `load-force.md` |
| Pressure Load | `load-pressure.md` |
| Centrifugal Load | `load-centrifugal.md` |
| Self Weight (Gravity) | `load-self-weight.md` |

### Thermal Constraints (split `constraints-thermal.md` → 4 files)

| Tool | Target file |
|------|-------------|
| Initial Temperature | `thermal-initial-temperature.md` |
| Heat Flux Load | `thermal-heat-flux.md` |
| Temperature Constraint | `thermal-temperature.md` |
| Body Heat Source | `thermal-body-heat-source.md` |

### Electromagnetic and Other Constraints (split `constraints-other.md` → 10 files)

| Tool | Target file |
|------|-------------|
| Electrostatic Potential | `em-electrostatic-potential.md` |
| Current Density | `em-current-density.md` |
| Magnetization | `em-magnetization.md` |
| Electric Charge Density | `em-electric-charge-density.md` |
| Initial Flow Velocity | `fluid-initial-flow-velocity.md` |
| Initial Pressure | `fluid-initial-pressure.md` |
| Flow Velocity Constraint | `fluid-flow-velocity.md` |
| Plane Rotation Constraint | `geom-plane-rotation.md` |
| Section Print | `geom-section-print.md` |
| Transform Constraint | `geom-transform.md` |

### Mesh (split `mesh.md` → 7 files)

| Tool | Target file |
|------|-------------|
| Mesh from Shape (Netgen) | `mesh-netgen.md` |
| Mesh from Shape (Gmsh) | `mesh-gmsh.md` |
| FEM Mesh Boundary Layer | `mesh-boundary-layer.md` |
| FEM Mesh Region | `mesh-region.md` |
| FEM Mesh Group | `mesh-group.md` |
| Create Elements Set | `mesh-elements-set.md` |
| Convert FEM Mesh to Mesh | `mesh-convert.md` |

### Solvers (split `solvers-and-equations.md` → 6 tool files + 1 concept page + N equation files)

The equation–solver–BC compatibility matrix is **concept content** →
`docs/fem/concepts/solver-equation-compatibility.md`

**Solver tool files:**

| Tool | Target file |
|------|-------------|
| Solver CalculiX | `solver-calculix.md` |
| Solver Elmer | `solver-elmer.md` |
| Solver Mystran | `solver-mystran.md` |
| Solver Z88 | `solver-z88.md` |
| Solver Job Control | `solver-job-control.md` |
| Run Solver | `run-solver.md` |

**Elmer Equations — special handling:**
Each Elmer equation is a separate FreeCAD command
(FEM → Solve → Elmer → Equations → …). Based on the FreeCAD source, the
equations available include at minimum: Deformation (Elasticity), Heat,
Flow (Navier-Stokes), Electrostatic, Magnetodynamic, Magnetodynamic 2D,
Flux, Electricforce, and Demagnetization.

Each equation should get its own file under `docs/fem/tools/`:

| Equation | Target file |
|----------|-------------|
| Deformation Equation | `eq-deformation.md` |
| Heat Equation | `eq-heat.md` |
| Flow Equation | `eq-flow.md` |
| Electrostatic Equation | `eq-electrostatic.md` |
| Magnetodynamic Equation | `eq-magnetodynamic.md` |
| Magnetodynamic 2D Equation | `eq-magnetodynamic-2d.md` |
| Flux Equation | `eq-flux.md` |
| Electricforce Equation | `eq-electricforce.md` |
| Demagnetization Equation | `eq-demagnetization.md` |

> ⚠️ **Verify the exact equation list against the FreeCAD source before writing.**
> The Python module `femsolver/elmer/equations/` is the authoritative list.
> Do not document equations that don't exist as distinct GUI commands.

### Results (split `results.md` → 3 files)

| Tool | Target file |
|------|-------------|
| Purge Results | `purge-results.md` |
| Show Results | `show-results.md` |
| VTK Post-Processing | `vtk-post-processing.md` |

**Files to delete:** `analysis.md`, `materials.md`, `element-geometry.md`,
`constraints-mechanical.md`, `constraints-thermal.md`, `constraints-other.md`,
`mesh.md`, `solvers-and-equations.md`, `results.md`

---

## Sprint 6 — Draft workbench

**Target: 6 merged files, ~64 tools + 16 snap modes**
**This is the largest and most complex sprint for a built-in workbench.**

### Design decision: Draft snapping modes

The `snapping.md` file documents 16 snap modes. Each mode has its own toolbar
button and can be toggled independently. Per the one-tool-per-file rule,
**each snap mode gets its own file.**

Additionally, there is a "Snap Lock" toggle (enables/disables the entire snapping
system) and a "Toggle Grid" command. Separate these too.

Create a concept page `docs/draft/concepts/snapping.md` that explains how the
snapping system works as a whole, what snap priority means, and how modes
interact. Each individual snap mode file links back to this concept page.

### Drafting tools (split `drafting.md` → 16 files)

| Tool | Target file |
|------|-------------|
| Line | `line.md` |
| Wire (Polyline) | `wire.md` |
| Fillet | `fillet.md` |
| Arc | `arc.md` |
| Arc by 3 Points | `arc-3-points.md` |
| Circle | `circle.md` |
| Ellipse | `ellipse.md` |
| Rectangle | `rectangle.md` |
| Polygon | `polygon.md` |
| B-Spline | `bspline.md` |
| Cubic Bézier Curve | `cubic-bezier.md` |
| Bézier Curve | `bezier.md` |
| Point | `point.md` |
| Facebinder | `facebinder.md` |
| ShapeString | `shape-string.md` |
| Hatch | `hatch.md` |

### Annotation tools (split `annotation.md` → 4 files)

| Tool | Target file |
|------|-------------|
| Text | `text.md` |
| Dimension | `dimension.md` |
| Label | `label.md` |
| Annotation Style Editor | `annotation-style-editor.md` |

### Modification tools (split `modification.md` → 19 files)

| Tool | Target file |
|------|-------------|
| Move | `move.md` |
| Rotate | `rotate.md` |
| Scale | `scale.md` |
| Mirror | `mirror.md` |
| Offset | `offset.md` |
| Trim / Extend | `trim-extend.md` |
| Stretch | `stretch.md` |
| Clone | `clone.md` |
| Edit | `edit.md` |
| Subelement Highlight | `subelement-highlight.md` |
| Join | `join.md` |
| Split | `split.md` |
| Upgrade | `upgrade.md` |
| Downgrade | `downgrade.md` |
| Wire to B-Spline | `wire-to-bspline.md` |
| Draft to Sketch | `draft-to-sketch.md` |
| Slope | `slope.md` |
| Flip Dimension | `flip-dimension.md` |
| Shape 2D View | `shape-2d-view.md` |

### Array tools (split `arrays.md` → 9 files)

| Tool | Target file |
|------|-------------|
| Ortho Array | `array-ortho.md` |
| Polar Array | `array-polar.md` |
| Circular Array | `array-circular.md` |
| Path Array | `array-path.md` |
| Path Link Array | `array-path-link.md` |
| Point Array | `array-point.md` |
| Point Link Array | `array-point-link.md` |
| Path Twisted Array | `array-path-twisted.md` |
| Path Twisted Link Array | `array-path-twisted-link.md` |

### Utility tools (split `utilities.md` → 16 files)

| Tool | Target file |
|------|-------------|
| Set Style | `set-style.md` |
| Apply Style | `apply-style.md` |
| Layer | `layer.md` |
| Layer Manager | `layer-manager.md` |
| Add Named Group | `add-named-group.md` |
| Select Group | `select-group.md` |
| Toggle Construction Mode | `toggle-construction-mode.md` |
| Add to Layer | `add-to-layer.md` |
| Add to Group | `add-to-group.md` |
| Add to Construction | `add-to-construction.md` |
| Toggle Display Mode | `toggle-display-mode.md` |
| Toggle Grid | `toggle-grid.md` |
| Select Working Plane | `select-working-plane.md` |
| Working Plane Proxy | `working-plane-proxy.md` |
| Heal | `heal.md` |
| Show Snap Bar | `show-snap-bar.md` |

### Snap mode tools (split `snapping.md` → 16 files + 1 concept page)

Individual snap mode files live in `docs/draft/tools/snap/`:

| Snap mode | Target file |
|-----------|-------------|
| Snap Lock | `snap/snap-lock.md` |
| Endpoint | `snap/snap-endpoint.md` |
| Midpoint | `snap/snap-midpoint.md` |
| Center | `snap/snap-center.md` |
| Angle | `snap/snap-angle.md` |
| Intersection | `snap/snap-intersection.md` |
| Perpendicular | `snap/snap-perpendicular.md` |
| Extension | `snap/snap-extension.md` |
| Parallel | `snap/snap-parallel.md` |
| Special | `snap/snap-special.md` |
| Near | `snap/snap-near.md` |
| Ortho | `snap/snap-ortho.md` |
| Grid | `snap/snap-grid.md` |
| Working Plane | `snap/snap-working-plane.md` |
| Dimensions | `snap/snap-dimensions.md` |
| Toggle Grid (Snap) | `snap/toggle-grid-snap.md` |

**Concept page:** `docs/draft/concepts/snapping.md`

**Files to delete:** `drafting.md`, `annotation.md`, `modification.md`,
`arrays.md`, `utilities.md`, `snapping.md`

---

## Sprint 7 — TechDraw workbench

**Target: 6 merged files, ~77 tools**
**Second largest sprint, complex extension pack structure.**

### Pages (split `pages.md` → 7 files)

| Tool | Target file |
|------|-------------|
| Insert Default Page | `insert-page-default.md` |
| Insert Page Using Template | `insert-page-template.md` |
| Fill Template Fields | `fill-template-fields.md` |
| Redraw Page | `redraw-page.md` |
| Print All Pages | `print-all-pages.md` |
| Export Page as SVG | `export-page-svg.md` |
| Export Page as DXF | `export-page-dxf.md` |

### Views (split `views.md` → 16 files)

| Tool | Target file |
|------|-------------|
| Insert View | `insert-view.md` |
| Insert Broken View | `insert-broken-view.md` |
| Insert Section View | `insert-section-view.md` |
| Insert Complex Section | `insert-complex-section.md` |
| Insert Detail View | `insert-detail-view.md` |
| Insert Projection Group | `insert-projection-group.md` |
| Insert Clip Group | `insert-clip-group.md` |
| Insert SVG Symbol | `insert-svg-symbol.md` |
| Insert Bitmap Image | `insert-bitmap-image.md` |
| Share View | `share-view.md` |
| Toggle Frames | `toggle-frames.md` |
| Project Shape | `project-shape.md` |
| Insert Active View | `insert-active-view.md` |
| Insert Draft View | `insert-draft-view.md` |
| Insert Arch View | `insert-arch-view.md` |
| Insert Spreadsheet View | `insert-spreadsheet-view.md` |

### Dimensions (split `dimensions.md` → 14 files)

| Tool | Target file |
|------|-------------|
| Insert Dimension | `dim-insert.md` |
| Insert Length Dimension | `dim-length.md` |
| Insert Horizontal Dimension | `dim-horizontal.md` |
| Insert Vertical Dimension | `dim-vertical.md` |
| Insert Radius Dimension | `dim-radius.md` |
| Insert Diameter Dimension | `dim-diameter.md` |
| Insert Angle Dimension | `dim-angle.md` |
| Insert 3-Point Angle Dimension | `dim-angle-3point.md` |
| Insert Area Annotation | `dim-area.md` |
| Insert Horizontal Extent Dimension | `dim-horizontal-extent.md` |
| Insert Vertical Extent Dimension | `dim-vertical-extent.md` |
| Repair Dimension References | `dim-repair-references.md` |
| Insert Balloon Annotation | `dim-balloon.md` |
| Insert Axonometric Length Dimension | `dim-axonometric-length.md` |

### Annotations (split `annotations.md` → 15 files)

| Tool | Target file |
|------|-------------|
| Insert Annotation | `annotation-text.md` |
| Insert Rich Text Annotation | `annotation-rich-text.md` |
| Insert Leader Line | `annotation-leader.md` |
| Hatch a Face | `hatch-face.md` |
| Apply Geometric Hatch | `hatch-geometric.md` |
| Add Face Centerline | `centerline-face.md` |
| Add Centerline Between 2 Lines | `centerline-2-lines.md` |
| Add Centerline Between 2 Points | `centerline-2-points.md` |
| Add Cosmetic Line Through 2 Points | `cosmetic-line.md` |
| Change Appearance of Lines | `change-line-appearance.md` |
| Show / Hide Invisible Edges | `toggle-invisible-edges.md` |
| Remove Cosmetic Objects | `remove-cosmetic.md` |
| Add Cosmetic Vertex | `cosmetic-vertex.md` |
| Add Midpoint Vertices | `midpoint-vertices.md` |
| Add Quadrant Vertices | `quadrant-vertices.md` |

### Symbols (split `symbols.md` → 3 files)

| Tool | Target file |
|------|-------------|
| Insert Welding Symbol | `symbol-welding.md` |
| Insert Surface Finish Symbol | `symbol-surface-finish.md` |
| Insert Hole / Shaft Fit | `symbol-hole-shaft-fit.md` |

### Extensions (split `extensions.md` → 22 files)

Extensions live in `docs/techdraw/tools/ext/`:

#### Attributes and Modifications
| Tool | Target file |
|------|-------------|
| Select Line Attributes | `ext/select-line-attributes.md` |
| Change Line Attributes | `ext/change-line-attributes.md` |
| Extend Line / Shorten Line | `ext/extend-shorten-line.md` |
| Lock / Unlock View | `ext/lock-unlock-view.md` |
| Position Section View | `ext/position-section-view.md` |

#### Centerlines and Threading
| Tool | Target file |
|------|-------------|
| Position Horizontal / Vertical / Oblique Chain | `ext/position-chain.md` |
| Cascade Horizontal / Vertical / Oblique Dimensions | `ext/cascade-dimensions.md` |
| Add Circle Centerlines | `ext/circle-centerlines.md` |
| Add Bolt Circle Centerlines | `ext/bolt-circle-centerlines.md` |
| Thread Representations | `ext/thread-representations.md` |
| Add Vertex at Intersection | `ext/vertex-at-intersection.md` |
| Add Offset Vertex | `ext/offset-vertex.md` |
| Cosmetic Circle / Arc Tools | `ext/cosmetic-circle-arc.md` |
| Draw Parallel / Perpendicular Line | `ext/parallel-perpendicular-line.md` |

#### Format and Organize Dimensions
| Tool | Target file |
|------|-------------|
| Calculate Area Annotation / Arc Length Annotation | `ext/area-arc-annotation.md` |
| Customise Dimension Format | `ext/customise-dimension-format.md` |
| Chain Dimensions | `ext/chain-dimensions.md` |
| Coordinate Dimensions | `ext/coordinate-dimensions.md` |
| Chamfer Dimensions | `ext/chamfer-dimensions.md` |
| Create Arc Length Dimension | `ext/arc-length-dimension.md` |
| Prefix Tools | `ext/prefix-tools.md` |
| Increase / Decrease Decimal Places | `ext/decimal-places.md` |

**Files to delete:** `pages.md`, `views.md`, `dimensions.md`, `annotations.md`,
`symbols.md`, `extensions.md`

---

## Sprint 8 — Curves Workbench

**Target: 3 merged files, 37 tools**
**Special consideration: third-party addon**

⚠️ The Curves Workbench is a **third-party addon** (not bundled with FreeCAD).
Add a standard notice block to every tool page in this workbench:

```markdown
!!! info "Third-party addon"
    Curves Workbench is not part of the standard FreeCAD installation.
    Install via the FreeCAD Addon Manager: Tools → Addon Manager → search
    "Curves". Requires FreeCAD 0.20 or later.
```

Also verify all tool names against the current addon source
(https://github.com/tomate44/CurvesWB) — third-party addons update independently
of FreeCAD and tool names or availability may have changed.

### Curve tools (split `curves.md` → 12 files)

| Tool | Target file |
|------|-------------|
| Parametric Line | `parametric-line.md` |
| Gordon Profile | `gordon-profile.md` |
| Mixed Curve | `mixed-curve.md` |
| Extend Curve | `extend-curve.md` |
| Join Curves | `join-curves.md` |
| Split Curve | `split-curve.md` |
| Discretize | `discretize.md` |
| Approximate | `approximate.md` |
| Interpolate | `interpolate.md` |
| Blend Curve | `blend-curve.md` |
| Comb Plot | `comb-plot.md` |
| Curve on Surface | `curve-on-surface.md` |

### Surface tools (split `surfaces.md` → 15 files)

| Tool | Target file |
|------|-------------|
| Trim Face | `trim-face.md` |
| IsoCurve | `iso-curve.md` |
| Sketch on Surface | `sketch-on-surface.md` |
| Map on Face | `map-on-face.md` |
| Sweep 2 Rails | `sweep-2-rails.md` |
| Pipeshell | `pipeshell.md` |
| Gordon Surface | `gordon-surface.md` |
| Segment Surface | `segment-surface.md` |
| Multi-Loft | `multi-loft.md` |
| Blend Surface | `blend-surface.md` |
| Blend Solid | `blend-solid.md` |
| Flatten Face | `flatten-face.md` |
| Rotation Sweep | `rotation-sweep.md` |
| Truncate / Extend | `truncate-extend.md` |
| Waterline Curves | `waterline-curves.md` |

### Analysis tools (split `analysis.md` → 10 files)

| Tool | Target file |
|------|-------------|
| Zebra Tool | `zebra.md` |
| Reflect Lines | `reflect-lines.md` |
| Surface Analysis | `surface-analysis.md` |
| Draft Analysis | `draft-analysis.md` |
| Geometry Info | `geometry-info.md` |
| Adjacent Faces | `adjacent-faces.md` |
| Extract Shapes | `extract-shapes.md` |
| Parametric Solid | `parametric-solid.md` |
| To Console | `to-console.md` |
| B-Spline to Console | `bspline-to-console.md` |

**Files to delete:** `curves.md`, `surfaces.md`, `analysis.md`

---

## Execution checklist (per sprint)

For each workbench sprint, follow this sequence in order:

1. **Write all new tool files** using `tool-documentation-format.md` as the template.
2. **Rewrite `tools/index.md`** as a pure navigation table (see Assembly's
   `tools/index.md` as the model).
3. **Create any new concept pages** identified in the sprint.
4. **Update `mkdocs.yml` nav** — replace the merged file entries with individual
   tool entries; add concept entries under a `Concepts:` subsection if applicable.
5. **Run `mkdocs build --strict`** — zero warnings required before proceeding.
6. **Delete the merged source files** — only after the build passes.
7. **Run `mkdocs build --strict` again** — confirm still zero warnings after deletion.

---

## Cross-linking standards

- Every tool page **See also** section must link to at least:
  - The workbench's `tools/index.md`
  - The closest prerequisite tool (e.g. a constraint tool links to the analysis
    creation tool)
  - The most relevant related tool (an alternative or complement)

- Concept pages are linked **from** tool pages, never the other way round
  (concept pages do not have See also links back to individual tools — that would
  make them tools indexes, which defeats the purpose).

---

## Special decisions log

| Topic | Decision | Rationale |
|-------|----------|-----------|
| Draft snap modes | Each snap mode gets its own `.md` file | Each has its own toolbar button → qualifies as a distinct tool per AP-1 definition |
| Draft snapping system overview | Concept page at `draft/concepts/snapping.md` | Explains the mechanism once; individual modes link to it |
| CAM common operation parameters | Concept page at `cam/concepts/operation-parameters.md` | Not a command — it's shared reference content |
| CAM feeds and speeds | Concept page at `cam/concepts/feeds-and-speeds.md` | Not a command |
| CAM tool bit files intro | Concept page at `cam/concepts/tool-bit-files.md` | Not a command |
| BIM IFC Import/Export overview | Concept page at `bim/concepts/ifc-import-export.md` | Not a command — it's a workflow description |
| FEM solver-equation compatibility matrix | Concept page at `fem/concepts/solver-equation-compatibility.md` | Not a command |
| FEM Elmer equations | Each equation gets its own `.md` file | Each is a distinct FreeCAD GUI command |
| Part `primitives.md` | Convert to concept/overview page | Individual primitive files already exist; this file is an overview, not a dump |
| Robot workbench deprecation | Add deprecation notice to every tool page | Robot WB is deprecated in FreeCAD 1.0; documentation should reflect this |
| Curves WB third-party status | Add addon installation notice to every tool page | Users need to know they must install the addon first |
| TechDraw extension tools | Each extension gets its own `.md` file in `ext/` sub-directory | Each has its own menu entry in TechDraw → Extensions |

---

## Total scope

| Metric | Count |
|--------|-------|
| Workbenches to remediate | 13 |
| Merged files to delete | 48 |
| New tool files to create | ~403 |
| New concept pages to create | ~12 |
| `mkdocs.yml` nav entries to update | ~450 |

This is a large but mechanical task. The Assembly sprint (18 files) took one
session. At that rate, the remaining 13 workbenches should take approximately
10–12 focused sessions.

**Recommended approach:** Complete one sprint per session. Each sprint is
self-contained — a passing `mkdocs build --strict` at the end of each sprint
means it's safe to stop and resume later.
