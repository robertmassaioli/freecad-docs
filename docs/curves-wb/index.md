# Curves Workbench

> **In one sentence:** The Curves workbench (CurvesWB) is a FreeCAD addon
> dedicated to advanced NURBS curve and surface operations — blending,
> trimming, Gordon surfaces, sweep-on-surface, surface analysis, and more
> — complementing the Surface workbench for complex free-form modelling.

---

## What is the Curves workbench for?

The Curves workbench extends FreeCAD's curve and surface capabilities beyond
what the built-in Surface workbench provides. It focuses on:

- **Advanced curve construction** — parametric line, curve extension, joining
  multiple edges into a single B-spline, splitting curves, B-spline interpolation
  and approximation.
- **Surface construction** — Gordon surface (from a curve network), blend surface
  between two edges, sweep on surface, multi-loft, pipeshell sweeps, rotation sweep.
- **Surface modification** — flatten conical/cylindrical faces, trim faces with
  projected curves, segment surfaces along iso-parameter lines, truncate/extend.
- **Analysis** — zebra stripe continuity check, comb plot (curvature display),
  draft angle analysis, surface curvature analysis, reflect lines, waterline curves.
- **Utilities** — sketch on surface (map a sketch to a curved face), map objects
  on a face, extract geometry, convert to console script.

The Curves workbench is an **external addon** — it is not included in the
FreeCAD installer. Install it via the **Addon Manager** (Tools → Addon Manager).

---

## Installation

1. Go to **Tools → Addon Manager**.
2. Search for **Curves**.
3. Click **Install**.
4. Restart FreeCAD.
5. The **Curves** workbench appears in the workbench selector.

---

## NURBS concepts used in this workbench

| Concept | Description |
|---------|-------------|
| **B-spline** | Piecewise polynomial curve defined by control points and knots |
| **NURBS** | Non-Uniform Rational B-Spline — B-spline with rational weights; can represent conics exactly |
| **Iso-curve** | A curve on a surface at constant U or V parameter |
| **Gordon surface** | A surface that exactly interpolates a network of crossing curves |
| **Blend surface** | A surface connecting two edges with a specified continuity (G0, G1, G2) |
| **G0 / G1 / G2 continuity** | Position / tangent / curvature continuity at a junction |
| **Zebra stripes** | Black-and-white stripe pattern that reveals G0/G1 discontinuities visually |
| **Comb plot** | Spines perpendicular to a curve, proportional to curvature, visualising curvature variation |
| **Draft angle** | Angle between a face normal and the mould release direction |

---

## Menu structure

The workbench adds three menus:

| Menu | Contents |
|------|----------|
| **Curves** | Parametric Line, Gordon Profile, Mixed Curve, Extend Curve, Join Curves, Split Curve, Discretize, Approximate, Interpolate, Blend Curve, Comb Plot, Curve on Surface |
| **Surfaces** | Zebra Tool, Trim Face, IsoCurve, Sketch on Surface, Map on Face, Sweep 2 Rails, Profile Support, Profile Sketch, Pipeshell, Gordon Surface, Segment Surface, Comp Spring, Reflect Lines, Multi-Loft, Blend Surface, Blend Solid, Flatten Face, Rotation Sweep, Surface Analysis, Draft Analysis, Truncate/Extend, Waterline Curves |
| **Misc.** | Geometry Info, Extract Shapes, Parametric Solid, Paste SVG, To Console, Adjacent Faces, B-Spline to Console |

---

## See also

- [Curve Tools](tools/curves.md) — all curve construction and modification tools
- [Surface Tools](tools/surfaces.md) — all surface construction and modification tools
- [Analysis and Utilities](tools/analysis.md) — analysis, inspection, and utility tools
- Surface workbench — built-in NURBS tools
