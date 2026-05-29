# Curves Workbench Tools

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Curve Tools

| Tool | Command | Description |
|------|---------|-------------|
| [Parametric Line](parametric-line.md) | `Curves_line` | Parametric line between two vertices |
| [Gordon Profile](gordon-profile.md) | `gordon_profile` | Editable profile curve for Gordon surfaces |
| [Mixed Curve](mixed-curve.md) | `mixed_curve` | 3-D curve as intersection of two projected curves |
| [Extend Curve](extend-curve.md) | `extend` | Extend an edge by a given distance |
| [Join Curves](join-curves.md) | `join` | Join selected edges into a single B-spline |
| [Split Curve](split-curve.md) | `split` | Split an edge at one or more points |
| [Discretize](discretize.md) | `Discretize` | Sample an edge or wire into a point array |
| [Approximate](approximate.md) | `Approximate` | Approximate a point set to a NURBS curve or surface |
| [Interpolate](interpolate.md) | `Interpolate` | Interpolate a NURBS curve through a point set |
| [Blend Curve](blend-curve.md) | `ParametricBlendCurve` | Blend curve between two edges with G1–G3 continuity |
| [Comb Plot](comb-plot.md) | `ParametricComb` | Visualise curvature as a comb on selected edges |
| [Curve on Surface](curve-on-surface.md) | `cos` | Create a curve constrained to lie on a surface |

---

## Surface Tools

| Tool | Command | Description |
|------|---------|-------------|
| [Trim Face](trim-face.md) | `Trim` | Trim a face with a projected curve |
| [IsoCurve](isocurve.md) | `IsoCurve` | Extract iso-parameter curves from a face |
| [Sketch on Surface](sketch-on-surface.md) | `SoS` | Map a sketch onto a curved surface |
| [Map on Face](map-on-face.md) | `Curves_MapOnFace` | Map objects onto a target face |
| [Sweep 2 Rails](sweep-2-rails.md) | `sw2r` | Sweep a profile along two guide rails |
| [Pipeshell](pipeshell.md) | `pipeshell` | Pipeshell sweep with orientation control |
| [Gordon Surface](gordon-surface.md) | `gordon` | Surface interpolating a network of crossing curves |
| [Segment Surface](segment-surface.md) | `segment_surface` | Divide a surface along iso-parameter lines |
| [Multi-Loft](multi-loft.md) | `MultiLoft` | Loft profiles composed of multiple faces |
| [Blend Surface](blend-surface.md) | `Curves_BlendSurf2` | Blend surface between two edges with continuity control |
| [Blend Solid](blend-solid.md) | `Curves_BlendSolid` | Blend solid between two faces |
| [Flatten Face](flatten-face.md) | `Curves_FlattenFace` | Unroll a conical or cylindrical face to a flat sheet |
| [Rotation Sweep](rotation-sweep.md) | `Curves_RotationSweep` | Sweep profiles around a centre point |
| [Truncate / Extend](truncate-extend.md) | `Curve_TruncateExtendCmd` | Cut shape at plane and truncate or extend |
| [Waterline Curves](waterline-curves.md) | `Curves_WaterlineCurves` | Generate horizontal waterline curves on a surface |

---

## Analysis and Utilities

| Tool | Command | Description |
|------|---------|-------------|
| [Zebra Tool](zebra-tool.md) | `ZebraTool` | Zebra stripe continuity analysis |
| [Reflect Lines](reflect-lines.md) | `ReflectLines` | Highlight reflection lines for a given view direction |
| [Surface Analysis](surface-analysis.md) | `Curves_SurfaceAnalysis` | Colour map of surface curvature |
| [Draft Analysis](draft-analysis.md) | `Curves_DraftAnalysis` | Colour map of draft angles for mould release |
| [Geometry Info](geometry-info.md) | `GeomInfo` | Show geometric properties of selected shapes |
| [Adjacent Faces](adjacent-faces.md) | `Curves_adjacent_faces` | Highlight faces adjacent to selected face |
| [Extract Shapes](extract-shapes.md) | `extract` | Extract sub-shapes from a compound |
| [Parametric Solid](parametric-solid.md) | `solid` | Create a solid from selected surfaces |
| [To Console](to-console.md) | `to_console` | Print shape properties to Python console |
| [B-Spline to Console](bspline-to-console.md) | `Curves_bspline_to_console` | Export B-spline control points to console |
