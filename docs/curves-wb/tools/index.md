# Curves Workbench Tools

## Curve Tools

| Tool | Command | Description |
|------|---------|-------------|
| [Parametric Line](curves.md#parametric-line) | `Curves_line` | Parametric line between two vertices |
| [Gordon Profile](curves.md#gordon-profile) | `gordon_profile` | Editable profile curve for Gordon surfaces |
| [Mixed Curve](curves.md#mixed-curve) | `mixed_curve` | 3D curve as intersection of two projected curves |
| [Extend Curve](curves.md#extend-curve) | `extend` | Extend an edge by a given distance |
| [Join Curves](curves.md#join-curves) | `join` | Join selected edges into a single B-spline |
| [Split Curve](curves.md#split-curve) | `split` | Split an edge at one or more points |
| [Discretize](curves.md#discretize) | `Discretize` | Sample an edge or wire into a point array |
| [Approximate](curves.md#approximate) | `Approximate` | Approximate a point set to a NURBS curve or surface |
| [Interpolate](curves.md#interpolate) | `Interpolate` | Interpolate a NURBS curve through a point set |
| [Blend Curve](curves.md#blend-curve) | `ParametricBlendCurve` | Blend curve between two edges with G0–G3 continuity |
| [Comb Plot](curves.md#comb-plot) | `ParametricComb` | Visualise curvature as a comb on selected edges |
| [Curve on Surface](curves.md#curve-on-surface) | `cos` | Create a curve constrained to lie on a surface |

## Surface Tools

| Tool | Command | Description |
|------|---------|-------------|
| [Trim Face](surfaces.md#trim-face) | `Trim` | Trim a face with a projected curve |
| [IsoCurve](surfaces.md#isocurve) | `IsoCurve` | Extract iso-parameter curves from a face |
| [Sketch on Surface](surfaces.md#sketch-on-surface) | `SoS` | Map a sketch onto a curved surface |
| [Map on Face](surfaces.md#map-on-face) | `Curves_MapOnFace` | Map objects onto a target face |
| [Sweep 2 Rails](surfaces.md#sweep-2-rails) | `sw2r` | Sweep a profile along two guide rails |
| [Pipeshell](surfaces.md#pipeshell) | `pipeshell` | PipeShell sweep with orientation control |
| [Gordon Surface](surfaces.md#gordon-surface) | `gordon` | Surface interpolating a network of crossing curves |
| [Segment Surface](surfaces.md#segment-surface) | `segment_surface` | Divide a surface along iso-parameter lines |
| [Multi-Loft](surfaces.md#multi-loft) | `MultiLoft` | Loft profiles composed of multiple faces |
| [Blend Surface](surfaces.md#blend-surface) | `Curves_BlendSurf2` | Blend surface between two edges with continuity control |
| [Blend Solid](surfaces.md#blend-solid) | `Curves_BlendSolid` | Blend solid between two faces |
| [Flatten Face](surfaces.md#flatten-face) | `Curves_FlattenFace` | Unroll a conical or cylindrical face to a flat sheet |
| [Rotation Sweep](surfaces.md#rotation-sweep) | `Curves_RotationSweep` | Sweep profiles around a centre point |
| [Truncate/Extend](surfaces.md#truncate-extend) | `Curve_TruncateExtendCmd` | Cut shape at plane and truncate or extend |
| [Waterline Curves](surfaces.md#waterline-curves) | `Curves_WaterlineCurves` | Generate horizontal waterline curves on a surface |

## Analysis and Utilities

| Tool | Command | Description |
|------|---------|-------------|
| [Zebra Tool](analysis.md#zebra-tool) | `ZebraTool` | Zebra stripe continuity analysis |
| [Reflect Lines](analysis.md#reflect-lines) | `ReflectLines` | Highlight reflection lines for a given view direction |
| [Surface Analysis](analysis.md#surface-analysis) | `Curves_SurfaceAnalysis` | Colour map of surface curvature |
| [Draft Analysis](analysis.md#draft-analysis) | `Curves_DraftAnalysis` | Colour map of draft angles for mould release |
| [Geometry Info](analysis.md#geometry-info) | `GeomInfo` | Show geometric properties of selected shapes |
| [Adjacent Faces](analysis.md#adjacent-faces) | `Curves_adjacent_faces` | Highlight faces adjacent to selected face |
| [Extract Shapes](analysis.md#extract-shapes) | `extract` | Extract sub-shapes from a compound |
| [Parametric Solid](analysis.md#parametric-solid) | `solid` | Create a solid from selected surfaces |
| [To Console](analysis.md#to-console) | `to_console` | Print shape properties to Python console |
| [B-Spline to Console](analysis.md#bspline-to-console) | `Curves_bspline_to_console` | Export B-spline control points to console |
