# Blend Curve

> **In one sentence:** Create a smooth NURBS curve that joins two edges with
> independently controlled G0–G3 geometric continuity at each end.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface → Blend Curve  
**Shortcut:** none

Generates a `Surface::FeatureBlendCurve` that connects a point on one edge to
a point on another edge. Continuity order (G0 through G3) and tangent influence
size are set independently at each end, giving fine control over how the
blending curve transitions into the adjacent geometry.

---

## Intuition

When two surfaces meet at an edge and you need a smooth transition curve
between them, Blend Curve creates exactly that connector. The continuity
settings determine how "smoothly" the curve arrives at each end:

- **G0** — the curve touches the edge endpoint (like two roads that meet at
  a junction but form a sharp corner).
- **G1** — the curve is tangent to the edge at the endpoint (the roads merge
  in a smooth, straight merge lane).
- **G2** — the curve matches the curvature at the endpoint (like a perfectly
  banked road where the curvature change is imperceptible).
- **G3** — the rate of curvature change also matches (the smoothest possible
  join, used in high-quality surface design).

The "size" parameter controls how far the tangent influence reaches — a larger
size creates a longer, more gradual transition.

---

## When to use it

- You need a fair transition curve to guide a Sections surface between two
  edges with specific continuity requirements.
- You are building a network of curves for Class-A surfacing and need blend
  curves at every join.
- You want to connect two edges with a curve that transitions smoothly into
  both without a visible kink.

---

## When NOT to use it

- **For filling a surface gap**, use [Filling](filling.md) — Blend Curve
  creates a curve, not a surface patch.
- **For a simple straight line between two points**, use Draft Line — the
  continuity machinery in Blend Curve would be unnecessary overhead.

---

## Step-by-step

1. Select two edges (Ctrl-click the second edge to add to selection).
2. Choose **Surface → Blend Curve**.
3. The blend curve appears connecting the nearest endpoints of the two edges.
4. In the Properties panel, set:
   - **Start Continuity** and **End Continuity** (0–3)
   - **Start Size** and **End Size** (tangent influence lengths)
   - **Start Parameter** and **End Parameter** (0.0–1.0) to move the
     attachment point along each edge.
5. Press **Enter** or recompute to see the updated curve.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Start Edge | Reference | — | First edge (the blend starts here) |
| Start Parameter | Float (0–1) | 1.0 | Position along the start edge where the blend attaches |
| Start Continuity | Integer (0–3) | 2 | Continuity order at the start: 0=G0, 1=G1, 2=G2, 3=G3 |
| Start Size | Float | 1.0 | Length of tangent influence at the start (larger = softer transition) |
| End Edge | Reference | — | Second edge (the blend ends here) |
| End Parameter | Float (0–1) | 0.0 | Position along the end edge |
| End Continuity | Integer (0–3) | 2 | Continuity order at the end |
| End Size | Float | 1.0 | Length of tangent influence at the end |

### Continuity order guide

| Order | Symbol | Visual effect |
|-------|--------|---------------|
| 0 | G0 | Meets the edge — sharp corner possible |
| 1 | G1 | Tangent — no visible kink |
| 2 | G2 | Curvature-matched — highlight flow is smooth |
| 3 | G3 | Acceleration-matched — smoothest possible join |

---

## Common mistakes and pitfalls

!!! warning "G2 or G3 continuity has no effect on a straight edge"
    **Cause:** Straight edges have zero curvature; G2 requires non-zero
    curvature to be meaningful.  
    **Fix:** Use G1 for edges adjacent to planar faces. G2/G3 work on curved
    NURBS edges.

!!! warning "Blend curve has an unexpected S-shape"
    **Cause:** The Start Size and End Size are too large relative to the
    distance between the edges — the curve overshoots and doubles back.  
    **Fix:** Reduce Start Size and End Size until the curve takes a direct
    path between the two edges.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
blend = doc.addObject("Surface::FeatureBlendCurve", "Blend")

blend.StartEdge = (doc.getObject("Body"), ["Edge1"])
blend.EndEdge   = (doc.getObject("Body"), ["Edge5"])
blend.StartParameter = 1.0   # end of Edge1
blend.EndParameter   = 0.0   # start of Edge5
blend.StartContinuity = 2    # G2
blend.EndContinuity   = 2    # G2
blend.StartSize = 10.0
blend.EndSize   = 10.0
doc.recompute()
```

---

## See also

- [Filling](filling.md) — fill a closed boundary with a surface
- [Sections](sections.md) — loft a surface through section curves
- [Extend Face](extend-face.md) — extrapolate a surface beyond its boundary
- [Surface Workbench](../index.md) — overview of the Surface workbench
