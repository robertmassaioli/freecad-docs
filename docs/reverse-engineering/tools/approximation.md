# Approximation

Approximation tools fit analytic or free-form surfaces to mesh or point cloud
data. The result in each case is a `Part` object that can be used in subsequent
CAD modelling.

---

## Plane {#plane}

**Menu:** Reverse Engineering → Approximation → Plane  
**Command:** `Reen_ApproxPlane`

Fits a **best-fit plane** to any geometric object (mesh, solid, or point cloud)
using a least-squares plane fit (PlaneFit algorithm). The result is a
`Part::Plane` positioned and oriented to minimise the sum of squared distances
from all input points to the plane.

The RMS (root mean square) residual is printed to the Report View:

```
RMS value for plane fit with N points: X.XXXX
```

Use this value as a quality indicator — a small RMS confirms the region is
genuinely planar.

### Result

A `Part::Plane` object is created with:
- Position at the corner of the bounding rectangle (not the centroid)
- Size (Length × Width) equal to the extent of the point set projected onto
  the plane
- Orientation aligned to the best-fit normal

### Step-by-step

1. Select one or more geometric objects (mesh, point cloud, or solid).
2. Go to **Reverse Engineering → Approximation → Plane**.
3. One `Part::Plane` is created per selected object.

---

## Cylinder {#cylinder}

**Menu:** Reverse Engineering → Approximation → Cylinder  
**Command:** `Reen_ApproxCylinder`

Fits a **best-fit cylinder** to a selected mesh. The algorithm estimates an
initial axis from the face normals, then refines the axis and radius by
iterative least-squares fitting.

### Result

A `Part::Cylinder` with:
- Radius = best-fit radius
- Height = length of the point set along the fit axis
- Placement aligned to the fit axis

### Prerequisites

Select a `Mesh::Feature`. The mesh segment should represent a cylindrical
region — a full mesh including non-cylindrical faces will produce an
inaccurate fit.

---

## Sphere {#sphere}

**Menu:** Reverse Engineering → Approximation → Sphere  
**Command:** `Reen_ApproxSphere`

Fits a **best-fit sphere** to a selected mesh.

### Result

A `Part::Sphere` with:
- Radius = best-fit radius
- Centre at the fit centroid

---

## Polynomial Surface {#polynomial-surface}

**Menu:** Reverse Engineering → Approximation → Polynomial Surface  
**Command:** `Reen_ApproxPolynomial`

Fits a **degree-2 Bézier surface** (a 3×3 control-point patch) to a mesh.
This produces a quadratic surface that fits planes, cylinders, spheres, cones,
paraboloids, and saddle surfaces within a single patch.

Internally, the algorithm:
1. Fits a polynomial z = f(x, y) of degree 2 to the mesh points.
2. Converts the polynomial to a 3×3 Bézier control grid.
3. Creates a `Part::Spline` (Geom_BezierSurface) document object.

### When to use vs B-Spline Surface

| Situation | Recommended tool |
|-----------|-----------------|
| Smooth, gently curved, single-patch region | Polynomial Surface |
| Complex curvature, large area, many patches | Approximate B-Spline Surface |
| Need parametric continuity to adjacent surfaces | Approximate B-Spline Surface |

---

## Approximate B-Spline Surface… {#approximate-b-spline-surface}

**Menu:** Reverse Engineering → Approximation → Approximate B-Spline Surface…  
**Command:** `Reen_ApproxSurface`

Fits a **free-form B-spline (NURBS) surface** to a point cloud or mesh. This
is the most powerful approximation tool in the workbench — it can fit any
smooth surface of arbitrary complexity.

### Task panel parameters

| Parameter | Description |
|-----------|-------------|
| **U degree** | Polynomial degree in the U parameter direction. Typical: 3 (cubic). |
| **V degree** | Polynomial degree in the V parameter direction. Typical: 3 (cubic). |
| **U control points** | Number of control points in U. More points → more flexibility → better fit, but risk of oscillation. |
| **V control points** | Number of control points in V. |
| **Iterations** | Number of refinement iterations. More → closer fit. |
| **Smoothing** | Smoothing weight. Higher → smoother surface, may deviate more from data. |
| **Continuity** | Internal continuity of the surface: C0, C1, or C2. |
| **Approximate order** | Order of the fitting approximation. |

### Accepted input

- `Points::Feature` — a point cloud (structured or unstructured)
- `Mesh::Feature` — a mesh (internally sampled to points)

### Result

A `Part::Spline` object containing the fitted NURBS surface. The surface can
be used in the Part workbench for further modelling (e.g. thickened into a
solid with Part → Offset 3D Surface).

### Guidelines for parameter selection

| Goal | Setting |
|------|---------|
| Smooth global surface with small deviations | High smoothing, moderate control points |
| Accurate fit to sharp features | Low smoothing, many control points |
| Avoid oscillation (Runge's phenomenon) | Cubic (degree 3) in both directions |
| Large region, complex shape | Increase U and V control points incrementally |

---

## Approximate B-Spline Curve… {#approximate-b-spline-curve}

**Menu:** Reverse Engineering → Approximation → Approximate B-Spline Curve…  
**Command:** `Reen_ApproxCurve`

Fits a **B-spline curve** to the points in a `Points::Feature`. The fitted
curve can serve as a boundary condition for the Surface workbench's
Filling or Sections tools, or as a guide rail for a Part Sweep.

### Task panel parameters

| Parameter | Description |
|-----------|-------------|
| **Degree** | Polynomial degree of the B-spline. Typical: 3 (cubic). |
| **Control points** | Number of control points. More → tighter fit to data. |
| **Continuity** | Internal continuity: C0, C1, C2. |
| **Closed** | Whether the resulting curve is a closed loop. |

### Prerequisites

Select a single `Points::Feature`. The points are projected onto the best-fit
parametric direction to define the arc-length parameter.

---

## Common mistakes and pitfalls

!!! warning "Cylinder / Sphere / Polynomial fit fails silently"
    **Cause:** The fitting algorithm returns `float::max` as the RMS when the
    fit cannot converge. No Part object is created.  
    **Fix:** Ensure the selected mesh segment actually corresponds to the target
    primitive type. Use **Mesh Segmentation** to isolate clean regions first.

!!! warning "B-Spline Surface has unwanted oscillations"
    **Cause:** Too many control points with too little smoothing causes the
    surface to chase noise in the data (over-fitting / Runge's phenomenon).  
    **Fix:** Reduce the number of control points, or increase the smoothing
    weight. Aim for the minimum number of control points that gives an
    acceptable RMS.

!!! warning "Plane fit produces a plane in the wrong orientation"
    **Cause:** The least-squares plane is unique only when the point cloud is
    non-degenerate. For a thin strip of points the normal can be ambiguous.  
    **Fix:** Check the RMS value in the Report View. If it is very small the
    fit is correct; if large, the region is not planar.

---

## See also

- [Surface Reconstruction](surface-reconstruction.md) — reconstruct a mesh from points first
- [Segmentation](segmentation.md) — isolate surface regions before fitting
- Surface workbench — free-form surface tools that consume the fitted geometry
