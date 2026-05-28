# Fill Boundary Curves

> **In one sentence:** Create a NURBS surface patch from exactly 2, 3, or 4
> boundary curves using the OpenCASCADE GeomFill algorithm.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface → Fill Boundary Curves  
**Shortcut:** none

Generates a smooth NURBS patch interpolated through 2, 3, or 4 boundary curves
(edges or wires). Three fill styles control the interior shape: Stretched,
Coons, and Curved. Simpler and faster than [Filling](filling.md) for patches
with a small number of boundary curves.

---

## Intuition

Think of the boundary curves as the four edges of a window frame (or three
edges of a triangular frame). Fill Boundary Curves stretches a smooth surface
sheet across that frame, fitting it to all four boundary curves simultaneously.

The three fill styles are different mathematical recipes for deciding how the
interior of the sheet behaves:

- **Stretched** minimises surface normal variation — produces the most uniform-
  looking surface.
- **Coons** uses the classic bilinear blending formula — a good all-round
  default for four-sided patches.
- **Curved** minimises curvature — produces the smoothest possible interior
  but may drift away from the boundary more than the other modes.

---

## When to use it

- You have a clean 2-, 3-, or 4-sided boundary of curves and want the fastest
  route to a surface patch.
- You are doing Class-A surfacing and need to choose between fill algorithms
  to see which one gives the best highlight reflection.
- You need a triangular (3-sided) patch — the Coons algorithm handles this
  naturally.

---

## When NOT to use it

- **More than 4 boundary curves** — use [Filling](filling.md) instead, which
  handles arbitrary closed loops.
- **When you need G1 or G2 continuity control** — Fill Boundary Curves only
  provides positional (G0) continuity at the boundary. Use Filling for
  continuity control.

---

## Step-by-step

1. Select exactly 2, 3, or 4 edges or wires (Ctrl+click).
2. Choose **Surface → Fill Boundary Curves**.
3. In the task panel, select the **Fill Style** (Stretched, Coons, or Curved).
4. Each curve appears in the list. If the surface appears twisted, click the
   reverse button (⇄) next to the offending curve to flip its direction.
5. Click **OK**. A `Surface::GeomFillSurface` object is created.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Boundary Edges | Reference list | — | 2, 3, or 4 boundary edges or wires |
| Fill Type | Enum | Stretched | Interior shape algorithm: Stretched, Coons, or Curved |

### Fill type in depth

| Style | Code | Best for |
|-------|------|----------|
| Stretched | 0 | Minimises normal variation — use for uniform-looking patches |
| Coons | 1 | Classic bilinear blend — good general-purpose choice for 4-sided patches |
| Curved | 2 | Minimises curvature — smoothest interior, may deviate from boundary shape |

---

## Common mistakes and pitfalls

!!! warning "Selecting 5 or more curves causes the tool to fail"
    **Cause:** The GeomFill algorithm is defined only for 2, 3, or 4 inputs.  
    **Fix:** Use [Filling](filling.md) for boundaries with more edges.

!!! warning "Surface appears twisted between two curves"
    **Cause:** One curve runs in the opposite direction to the adjacent curve,
    causing the surface to wrap around itself.  
    **Fix:** Use the reverse button (⇄) in the task panel to flip the
    offending curve's direction.

!!! warning "Curves don't connect at corners"
    **Cause:** The boundary curves do not share endpoints (there are gaps).
    GeomFill requires connected boundary curves.  
    **Fix:** Close the gaps in the boundary before applying. Add connecting
    segments with Draft or use the Sketcher to ensure end-to-end connectivity.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
surf = doc.addObject("Surface::GeomFillSurface", "FillSurface")

# Set boundary edges — list of (object, [edge_name]) tuples
surf.BoundaryEdges = [
    (doc.getObject("Wire1"), ["Edge1"]),
    (doc.getObject("Wire2"), ["Edge1"]),
    (doc.getObject("Wire3"), ["Edge1"]),
    (doc.getObject("Wire4"), ["Edge1"]),
]

# Fill type: 0=Stretched, 1=Coons, 2=Curved
surf.FillType = 1   # Coons
doc.recompute()
```

---

## See also

- [Filling](filling.md) — handles more than 4 boundary edges and provides G1/G2 control
- [Sections](sections.md) — loft through multiple cross-section profiles
- [Surface Workbench](../index.md) — overview of the Surface workbench
