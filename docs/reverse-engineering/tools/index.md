# Reverse Engineering Tools

Complete reference for all Reverse Engineering workbench tools.

---

## Surface Reconstruction

| Tool | Description |
|------|-------------|
| [Poisson Reconstruction](poisson-reconstruction.md) | Reconstruct a watertight mesh from a point cloud with normals |
| [Structured Point Clouds](structured-point-clouds.md) | Triangulate a structured (grid) point cloud directly into a mesh |

---

## Segmentation

| Tool | Description |
|------|-------------|
| [Mesh Segmentation](mesh-segmentation.md) | Auto-split a mesh into regions by surface type using curvature analysis |
| [Manual Segmentation](manual-segmentation.md) | Paint mesh regions interactively with a brush |
| [From Components](from-components.md) | Create one segment per disconnected mesh component |
| [Wire From Mesh Boundary](wire-from-mesh-boundary.md) | Extract mesh boundary loops as Part wires or a face |

---

## Approximation

| Tool | Description |
|------|-------------|
| [Approximate Plane](approx-plane.md) | Fit a best-fit plane to geometry |
| [Approximate Cylinder](approx-cylinder.md) | Fit a best-fit cylinder to a mesh |
| [Approximate Sphere](approx-sphere.md) | Fit a best-fit sphere to a mesh |
| [Approximate Polynomial Surface](approx-polynomial-surface.md) | Fit a degree-2 Bézier (quadric) patch to a mesh |
| [Approximate B-Spline Surface](approx-bspline-surface.md) | Fit a free-form NURBS surface to a point cloud or mesh |
| [Approximate B-Spline Curve](approx-bspline-curve.md) | Fit a B-spline curve to a point cloud |
