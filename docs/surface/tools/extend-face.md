# Extend Face

> **In one sentence:** Extrapolate a selected face beyond its current boundary
> by extending it along its parametric U and V directions.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface → Extend Face  
**Shortcut:** none

Takes a single face from a solid or surface body and extrapolates it beyond
its boundary edges. The extension is tangent to the original surface at the
join. Used to make surfaces overlap for subsequent trimming operations.

---

## Intuition

A NURBS surface is defined by a mathematical function over a rectangular
parameter domain. That domain can be extended beyond its current boundaries
simply by evaluating the function at parameter values outside the original
range. Extend Face does exactly this — it evaluates the surface function
slightly outside the current domain to produce an extended surface.

Think of it like pulling the edge of a rubber sheet: the sheet stretches
beyond its original boundary, staying tangent at the pull point. The farther
you pull, the more the extended portion may deviate from the "natural"
extrapolation.

---

## When to use it

- You need a surface to overlap another surface so you can trim them together
  to a clean intersection line.
- A surface doesn't quite reach an edge it should meet, and you need to push
  it past that edge before trimming.
- You want to see how a surface's mathematical shape continues beyond its
  current boundary.

---

## When NOT to use it

- **For large extensions** — NURBS extrapolation degrades quickly with
  distance. Extensions larger than 10–20% of the surface size often produce
  unexpected curling or looping. Use a dedicated surface extension approach
  (creating new geometry) for large extensions.
- **For adding material to a solid** — this produces a surface, not a solid
  feature. Use Part Design tools for solid operations.

---

## Step-by-step

1. Select a single face on a solid or surface object.
2. Choose **Surface → Extend Face**.
3. A `Surface::Extend` object is created and the extended surface appears,
   overlapping the original.
4. In the Properties panel, adjust **Tolerance** if needed.
5. Drag the boundary handles in the 3-D view (if available) to control
   the extension distance.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Face | Reference | — | The source face to extend |
| Tolerance | Float | 0.1 | Maximum tangency deviation at the extension join |

---

## Common mistakes and pitfalls

!!! warning "Extension curls, loops, or has extreme curvature"
    **Cause:** NURBS extrapolation is mathematically valid but not
    geometrically predictable for large extension distances.  
    **Fix:** Keep extensions small (5–10% of the surface dimension). For
    larger extensions, build new geometry rather than extrapolating.

!!! warning "Cannot select an interior face"
    **Cause:** Face selection requires clicking directly on the face in the
    3-D view; clicking on an edge selects the edge, not the face.  
    **Fix:** Click clearly in the interior of the face, away from edges.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument

ext = doc.addObject("Surface::Extend", "ExtendFace")
ext.Face = (doc.getObject("Body"), ["Face6"])  # replace with target face
ext.Tolerance = 0.01
doc.recompute()
```

---

## See also

- [Filling](filling.md) — close a gap with a proper fill surface
- [Blend Curve](blend-curve.md) — smooth blending curve at an edge
- [Surface Workbench](../index.md) — overview of the Surface workbench
