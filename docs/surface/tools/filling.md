# Filling

> **In one sentence:** Create a smooth NURBS surface that fills a closed
> boundary of selected edges with controlled G0, G1, or G2 continuity.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface → Filling  
**Shortcut:** none

Fills a closed loop of edges with a parametric NURBS surface. Boundary
continuity can be set independently per edge (G0 = touching, G1 = tangent,
G2 = curvature-matched). Internal edges and vertices can pull the fill shape
towards desired interior geometry.

---

## Intuition

Imagine stretching a soap film across a wire frame that you've bent into a
closed loop. The soap film fills the hole with the smoothest possible surface
that touches every wire. Filling does the same thing mathematically: given a
closed boundary of edges, it solves for the smoothest NURBS patch that satisfies
all boundary conditions.

The "continuity" settings control whether the patch just touches the adjacent
face (G0), lies tangent to it (G1, like two sheets of paper meeting in a flat
join), or matches its curvature (G2, like a perfectly blended rolling hill with
no visible crease).

---

## When to use it

- You need to close an open region on a surface model with a smooth patch
  (e.g. filling the top face of a complex bottle shape).
- You need G1 or G2 continuity across the boundary — for aesthetic product
  surfaces where highlights must flow smoothly across joins.
- You have more than 4 boundary edges (Fill Boundary Curves only handles 2–4).

---

## When NOT to use it

- **For 2, 3, or 4 boundary curves**, consider [Fill Boundary Curves](fill-boundary-curves.md)
  first — it is simpler to use and uses a different algorithm that may give
  better results for simple patches.
- **For solid filling**, use Part Design Pad or Part Loft — Filling produces
  a surface, not a solid.

---

## Step-by-step

1. Select a closed boundary loop of edges (hold Ctrl to select multiple edges).
   Every edge must share an endpoint with the next.
2. Choose **Surface → Filling**.
3. In the task panel under **Boundary**, verify the listed edges.
4. For each boundary edge, set:
   - **Order** — polynomial degree along that edge (3 = cubic, 5 = quintic)
   - **Continuity** — None (G0), G1, or G2
5. Optionally switch to the **Unbound** tab and add internal edges or vertices
   to constrain the interior shape.
6. Click **OK**. A `Surface::Filling` object appears in the model tree.

!!! tip
    Start with G1 continuity on all edges. Switch to G2 only if the adjacent
    faces are smooth NURBS surfaces — G2 on a planar face will be ignored.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Boundary Edges | Reference list | — | Edges forming the closed boundary loop |
| Order | Integer (per edge) | 3 | Polynomial degree along boundary edge (higher = can match more complex boundary curvature) |
| Continuity | Enum (per edge) | G0 | Tangency condition at this edge: None (G0), G1 (tangent), G2 (curvature) |
| Max Degree | Integer | 5 | Maximum NURBS degree of the result surface |
| Max Segments | Integer | 9 | Maximum number of NURBS spans per boundary edge |
| Internal Edges | Reference list | — | Edges inside the boundary that guide the surface interior |
| Internal Vertices | Reference list | — | Points the surface must pass through |

### Continuity in depth

| Mode | What it enforces | Requires |
|------|-----------------|----------|
| G0 (None) | Surface passes through the boundary edge | Nothing special |
| G1 | Surface is tangent to the adjacent face | Adjacent face must exist |
| G2 | Surface matches curvature of the adjacent face | Adjacent face must be a smooth (non-planar) NURBS surface |

---

## Common mistakes and pitfalls

!!! warning "Filling fails: boundary is not a closed loop"
    **Cause:** One or more edges have a gap at their junction — the endpoint
    coordinates do not match exactly.  
    **Fix:** Use Part → Check Geometry to find gaps. Add short connecting
    edges with the Draft or Sketcher workbench to close the loop.

!!! warning "G2 continuity has no visible effect"
    **Cause:** G2 requires the adjacent face to have a well-defined curvature.
    Planar faces (curvature = 0 everywhere) cannot donate curvature to the fill.  
    **Fix:** Use G1 for boundaries adjacent to planar faces; reserve G2 for
    boundaries on curved freeform surfaces.

!!! warning "Surface looks faceted, not smooth"
    **Cause:** Max Degree or Max Segments is too low for the complexity of the
    boundary.  
    **Fix:** Increase Max Degree (up to 8) and Max Segments to give the solver
    more freedom.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument

# Create the filling surface object
surf = doc.addObject("Surface::Filling", "Fill")

# Set boundary edges — list of (object, [edge names]) tuples
body = doc.getObject("Body")
surf.BoundaryEdges = [
    (body, ["Edge1"]),
    (body, ["Edge2"]),
    (body, ["Edge3"]),
    (body, ["Edge4"]),
]
surf.BoundaryOrder = [3, 3, 3, 3]     # cubic along each edge
# Continuity: 0=G0, 1=G1, 2=G2
surf.BoundaryFaces = [...]             # TODO: verify exact continuity API

surf.MaxDegree = 5
surf.MaxSegments = 9
doc.recompute()
```

---

## See also

- [Fill Boundary Curves](fill-boundary-curves.md) — simpler tool for 2–4 curve boundaries
- [Sections](sections.md) — loft a surface through cross-section edges
- [Extend Face](extend-face.md) — extrapolate an existing surface beyond its boundary
- [Surface Workbench](../index.md) — overview of the Surface workbench
