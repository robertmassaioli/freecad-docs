# Surface Tools

> **In one sentence:** The Surface tools create smooth NURBS patches from
> selected boundary edges and curves, extend existing surfaces, and
> project curves onto meshes.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface

| Tool | Description |
|------|-------------|
| [Filling](#filling) | Fill a boundary with a smooth NURBS surface |
| [Fill Boundary Curves](#fill-boundary-curves) | NURBS patch from 2, 3, or 4 boundary curves |
| [Sections](#sections) | Loft through cross-section edges |
| [Extend Face](#extend-face) | Extrapolate a surface beyond its boundary |
| [Blend Curve](#blend-curve) | Smooth blending curve between two edges |
| [Curve on Mesh](#curve-on-mesh) | Project a curve onto a mesh |

---

## Filling

**Menu:** Surface → Filling  
**Command:** `Surface_Filling`

Creates a smooth NURBS surface that fills a closed boundary region. The
boundary is defined by selecting edges of existing geometry. Additional
internal edges and vertices can constrain the shape of the fill patch.

### Boundary edges

The boundary must be a closed loop — a set of connected edges that form
a complete perimeter. All boundary edges must share endpoints (no gaps).

**Boundary continuity** — each boundary edge can be assigned one of:

| Continuity | Symbol | Effect |
|------------|--------|--------|
| G0 | C0 | Fill meets boundary edge — no gap |
| G1 | C1 | Fill is tangent to the adjacent face at the boundary |
| G2 | C2 | Fill matches the curvature of the adjacent face |

Higher continuity produces a smoother join. G2 requires that the adjacent
face itself has a well-defined curvature field (i.e. is a smooth surface,
not a planar face).

### Internal constraints

Beyond the boundary, you can add:

- **Internal edges** — edges inside the boundary that the surface must
  pass through or near.
- **Internal vertices** — points the surface must pass through.

These constrain the interior shape of the fill without changing the
boundary. Useful for pushing a fill towards a specific control surface.

### Step-by-step

1. Select a closed boundary loop of edges (hold Ctrl to select multiple
   edges).
2. Choose **Surface → Filling**.
3. In the task panel:
   - Under **Boundary**, verify the edges are listed.
   - Set the **Order** (surface degree: 3 = cubic, 5 = quintic) and
     **Max Segments** for each boundary edge.
   - Set the continuity (None / G1 / G2) per edge.
4. Optionally add internal edges and vertices under the **Unbound** tab.
5. Click **OK**. A `Surface::Filling` object appears in the model tree.

### Key properties

| Property | Description |
|----------|-------------|
| Boundary Edges | Reference edges forming the closed boundary |
| Boundary Order | Polynomial degree along boundary edges |
| Boundary Points | Inner edges that guide the surface interior |
| Unbound Vertices | Points the surface must pass through |
| Max Degree | Maximum NURBS degree of the result surface |
| Max Segments | Maximum number of NURBS segments per boundary edge |

### Python API

```python
import Surface

doc = App.ActiveDocument
edge1 = doc.getObject("Body").Shape.Edges[0]
# ... select boundary edges

surf = doc.addObject("Surface::Filling", "Fill")
surf.BoundaryEdges = [(doc.getObject("Body"), ["Edge1", "Edge2", "Edge3"])]
doc.recompute()
```

---

## Fill Boundary Curves

**Menu:** Surface → Fill Boundary Curves  
**Command:** `Surface_GeomFillSurface`

Creates a NURBS surface patch from **2, 3, or 4** boundary curves (edges
or wires). Unlike the general Filling tool, this uses the OpenCASCADE
`GeomFill` algorithm which works specifically with 2, 3, or 4 wire
boundaries.

### Supported configurations

| Boundary count | Result |
|---------------|--------|
| 2 curves | Ruled surface between the two curves |
| 3 curves | Triangular NURBS patch |
| 4 curves | Quadrilateral NURBS patch (bilinear interpolation) |

### Fill style

The interpolation style controls how the surface is computed in the
interior:

| Style | Description |
|-------|-------------|
| Stretched | Minimises the variation of the surface normals — more uniform |
| Coons | Classic Coons patch — bilinear blending of the boundary conditions |
| Curved | Minimises surface curvature — smoother but may deviate more from the boundary |

**Coons** is the classic choice for 4-sided patches. **Stretched** works
well for 3-sided (triangular) patches.

### Step-by-step

1. Select 2, 3, or 4 edges or wires (hold Ctrl).
2. Choose **Surface → Fill Boundary Curves**.
3. In the task panel, select the fill style.
4. Each curve appears in the list — use the reverse button (⇄) to flip
   the direction of a curve if the surface is twisted.
5. Click **OK**.

### When curves don't connect at corners

Fill Boundary Curves requires the boundary curves to be connected
(their endpoints shared or coincident). If there are gaps, the surface
will either fail to compute or produce a distorted result.

Close all gaps in the boundary before applying this tool. Use Draft
workbench to add short connecting segments if needed.

### Python API

```python
import Surface

doc = App.ActiveDocument
surf = doc.addObject("Surface::GeomFillSurface", "FillSurface")
surf.BoundaryEdges = [
    (doc.getObject("Wire1"), ["Edge1"]),
    (doc.getObject("Wire2"), ["Edge1"]),
    (doc.getObject("Wire3"), ["Edge1"]),
    (doc.getObject("Wire4"), ["Edge1"]),
]
surf.FillType = 0  # 0=Stretched, 1=Coons, 2=Curved
doc.recompute()
```

---

## Sections

**Menu:** Surface → Sections  
**Command:** `Surface_Sections`

Creates a lofted NURBS surface through a series of cross-section edges
or wires. Each section is a profile; the tool interpolates a smooth
surface through all sections in order.

### The lofting concept

Sections works like Part Loft but produces a NURBS surface (not a solid)
and accepts edges (not just sketches) as section profiles. This is
useful for surfacing from existing geometry.

**Profile requirements:**
- Sections should have the same number of edges or be continuous wires.
- Sections should be oriented consistently (same direction).
- The first and last sections may optionally be tangent-constrained
  to adjacent surfaces (G1 continuity).

### Step-by-step

1. Select the cross-section edges in sequence (first to last).
2. Choose **Surface → Sections**.
3. In the task panel:
   - Verify the section list. Use the up/down arrows to reorder if needed.
   - For each section edge, use the reverse button to flip direction.
4. Click **OK**.

### Key properties

| Property | Description |
|----------|-------------|
| NSections | The list of section edges/wires in order |
| Tangent Edge | First and last tangent edge for G1 boundary continuity |

### Compared to Part Loft

| Feature | Surface Sections | Part Loft |
|---------|-----------------|-----------|
| Input | Edges and wires | Sketches, wires |
| Output | NURBS surface | Solid shell |
| Tangent continuity | Yes (edges) | Yes (profiles) |
| Closed result | No | Optional |
| Use for | Surface modelling | Solid creation |

---

## Extend Face

**Menu:** Surface → Extend Face  
**Command:** `Surface_ExtendFace`

Extrapolates a selected face or surface beyond its current boundary by
extending it in the local U and V parametric directions.

### How it works

NURBS surfaces are parameterised in U and V — two orthogonal directions
across the surface. The surface can be extended beyond its current
parameter range by extrapolating the mathematical function.

The extension is **tangent** to the original surface at the boundary
(G1 continuity). The farther the extension goes, the more the extrapolated
shape may deviate from the "natural" continuation.

### Step-by-step

1. Select a single face on a solid or surface.
2. Choose **Surface → Extend Face**.
3. In the Properties panel, adjust:
   - **Tolerance** — how much the extension deviates from G1
   - (The extension length is set by dragging the boundary handles in
     the 3-D view.)
4. The extended surface appears, overlapping the original.

### Key properties

| Property | Description |
|----------|-------------|
| Face | The source face to extend |
| Tolerance | Maximum allowed tangency deviation at the join |

### Use cases

- Extending a surface to ensure it overlaps another surface for trimming.
- Creating a cap surface that extends slightly beyond the solid body
  before being trimmed to the exact boundary.
- Checking how a surface "wants" to continue beyond its current extent.

---

## Blend Curve

**Menu:** Surface → Blend Curve  
**Command:** `Surface_BlendCurve`

Creates a smooth **blending curve** (a NURBS curve) that joins two
edges with controlled geometric continuity at each end.

### Continuity control

At each end of the blend curve, you can set the continuity order
independently:

| Continuity | Meaning | Value |
|------------|---------|-------|
| G0 | Position only — meets the edge endpoint | 0 |
| G1 | Tangent continuity — matches the tangent direction | 1 |
| G2 | Curvature continuity — matches curvature radius and direction | 2 |
| G3 | Acceleration continuity — smoothest possible join | 3 |

Higher continuity produces a smoother visual transition but gives the
curve less freedom to find an attractive shape.

### Parameters

| Property | Description |
|----------|-------------|
| Start Edge | First edge (the blend starts here) |
| Start Parameter | Position along the start edge (0.0–1.0) |
| Start Continuity | Continuity order at the start (0–3) |
| Start Size | Length of the tangent influence at the start |
| End Edge | Second edge (the blend ends here) |
| End Parameter | Position along the end edge (0.0–1.0) |
| End Continuity | Continuity order at the end (0–3) |
| End Size | Length of the tangent influence at the end |

**Size** controls how much the blend curve is "pulled" towards the
adjacent surface in the tangent direction. A larger size creates a
softer, more gradual transition; a smaller size creates a tighter blend.

### Step-by-step

1. Select two edges (Ctrl-click to select the second).
2. Choose **Surface → Blend Curve**.
3. The blend curve appears connecting the two edge endpoints.
4. In the Properties panel, adjust Start/End Continuity and Size.
5. Recompute to see the result.

### Python API

```python
import Surface

doc = App.ActiveDocument
blend = doc.addObject("Surface::FeatureBlendCurve", "Blend")
blend.StartEdge = (doc.getObject("Part"), ["Edge1"])
blend.EndEdge = (doc.getObject("Part"), ["Edge5"])
blend.StartParameter = 1.0    # end of Edge1
blend.EndParameter = 0.0      # start of Edge5
blend.StartContinuity = 2     # G2
blend.EndContinuity = 2       # G2
blend.StartSize = 10.0
blend.EndSize = 10.0
doc.recompute()
```

---

## Curve on Mesh

**Menu:** Surface → Curve on Mesh  
**Command:** `Surface_CurveOnMesh`

Creates an approximated B-spline curve on the surface of a mesh object.
The curve follows the mesh surface, snapping to it as you pick points.

### Use case: reverse engineering

When you have an imported scan mesh (STL) and want to extract curves
for surface reconstruction:

1. Import the mesh (Mesh workbench → Import).
2. Switch to Surface workbench.
3. Activate **Curve on Mesh**.
4. Pick points on the mesh surface — each click places a control point.
5. The resulting curve follows the mesh surface.
6. Use the extracted curves as boundary edges for Filling or Sections.

### Step-by-step

1. Select the mesh object.
2. Choose **Surface → Curve on Mesh**.
3. Pick points on the mesh surface (they snap to the mesh).
4. Press **Enter** or right-click to finish the curve.
5. Optionally adjust the **Interpolation** parameters to control how
   closely the B-spline follows the picked points.

### Key parameters

| Parameter | Description |
|-----------|-------------|
| Mesh | The source mesh object |
| Wire | The B-spline curve draped on the mesh |
| Segment Size | Approximation tolerance in mm |
| Angle Tolerance | Maximum angle between consecutive mesh normals for a curve segment |

!!! note "MeshPart module"
    Curve on Mesh is technically part of the MeshPart module (not the
    Surface module's App code), accessible through the Surface workbench UI.

---

## Common mistakes and pitfalls

!!! warning "Filling boundary must be a closed loop"
    The Filling tool requires all boundary edges to form a single closed
    loop — every edge must connect end-to-end with the next. If there are
    gaps between edges, the surface cannot be computed. Check the boundary
    carefully with **Part → Check Geometry** if filling fails.

!!! warning "Fill Boundary Curves requires exactly 2, 3, or 4 curves"
    GeomFillSurface strictly requires 2, 3, or 4 boundary curves. If you
    select 5 or more, the tool refuses to proceed. Use the Filling tool
    for boundaries with more than 4 edges.

!!! warning "Extend Face extrapolation quality degrades with distance"
    NURBS extrapolation is mathematically valid but not physically
    meaningful for large extensions. A surface extrapolated 50 mm beyond
    its boundary may curl, loop back, or create extreme curvature. Use
    small extensions (5–10% of the surface size) for reliable results.

!!! warning "Blend Curve with G2/G3 continuity requires smooth adjacent surfaces"
    G2 (curvature) continuity at an edge requires that the face adjacent
    to that edge has a well-defined, smooth curvature field. Planar faces
    (curvature = 0) can only be blended at G0 or G1. For G2 blends, both
    edges must be on curved (non-planar) NURBS surfaces.

!!! warning "Sections loft can twist if profiles are not consistently oriented"
    If one section profile runs clockwise and the next runs counter-clockwise,
    the lofted surface will twist between them. Use the reverse button (⇄)
    in the task panel to flip profile orientations until they all run in the
    same direction.

---

## See also

- [Surface Workbench Overview](../index.md) — continuity types, NURBS concepts
- Part Loft — solid lofting through profiles
- Part Design Additive Loft — solid loft feature in Part Design
- Mesh workbench — import and work with mesh data for Curve on Mesh
