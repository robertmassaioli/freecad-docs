# Surface Workbench

> **In one sentence:** The Surface workbench creates smooth, continuous
> surfaces by filling boundary curves — essential for Class-A surfacing,
> complex organic shapes, and surface modelling that goes beyond what
> Part Design's solid-only workflow can achieve.

---

## What is the Surface workbench for?

The Surface workbench creates **NURBS surfaces** from curves and edges.
It bridges the gap between sketch-based parametric modelling (Part Design)
and the curve-driven surface modelling used in automotive, consumer
product, and industrial design.

**Common uses:**

- Filling the open face of a solid body with a smooth curved patch.
- Creating transitions between two surfaces with controlled continuity
  (tangent-smooth or curvature-smooth joins).
- Generating a surface from a set of cross-section curves (lofting).
- Extending a surface beyond its current boundaries.
- Projecting curves onto a mesh for reverse engineering.

---

## Key concepts

### NURBS surfaces

All Surface workbench tools produce **NURBS** (Non-Uniform Rational
B-Spline) surfaces. A NURBS surface is defined by a control point grid
and associated weights — it can represent any smooth curved shape exactly
or to arbitrary precision.

NURBS surfaces are fully compatible with Part and Part Design — you can
combine them with solids using boolean operations and Part features.

### Continuity types

When two surfaces meet at a shared boundary, there are several levels of
geometric continuity:

| Continuity | Symbol | Meaning |
|------------|--------|---------|
| Position | G0 / C0 | Surfaces touch — no gap, but may have a crease |
| Tangent | G1 / C1 | Surfaces are tangent — no crease, but curvature may jump |
| Curvature | G2 / C2 | Curvature matches across the boundary — smooth highlight lines |
| Acceleration | G3 / C3 | Rate of change of curvature matches — needed for automotive |

**G0** is sufficient for watertight geometry.  
**G1** is required for visually smooth shading.  
**G2** is required for Class-A surfacing (automotive, consumer products) where
highlight reflections must flow smoothly across panel joins.

---

## Workbench layout

The Surface workbench has one menu (**Surface**) with six tools:

| Tool | Description |
|------|-------------|
| Filling | Fill a boundary region with a smooth surface |
| Fill Boundary Curves | Fill 2–4 boundary curves with a NURBS patch |
| Sections | Loft a surface through cross-section curves |
| Extend Face | Extrapolate a surface beyond its current boundary |
| Curve on Mesh | Project a sketch curve onto a mesh surface |
| Blend Curve | Join two edges with a smooth blending curve |

---

## See also

- [Tools](tools/index.md) — all Surface tools with full detail
- Part Design Pad/Loft — solid features that can use Surface patches as caps
- Part workbench — Boolean operations that combine surfaces with solids
