# Reverse Engineering Workbench

> **In one sentence:** The Reverse Engineering workbench converts scanned
> data — point clouds and meshes — into editable CAD geometry by fitting
> analytic primitives (planes, cylinders, spheres), B-spline surfaces and
> curves, segmenting meshes by surface type, and reconstructing closed surfaces
> via Poisson reconstruction.

---

## What is the Reverse Engineering workbench for?

Reverse engineering in FreeCAD means taking data from the physical world
(a 3-D scan, an imported mesh, or a point cloud) and turning it back into
precise CAD geometry. The Reverse Engineering workbench provides:

- **Surface reconstruction** — build a watertight mesh from an unordered
  point cloud with normals (Poisson) or triangulate an ordered depth-camera
  scan directly.
- **Mesh segmentation** — split a complex mesh into regions corresponding to
  recognisable surface types (plane, cylinder, sphere, B-spline patch) so
  each region can be fitted independently.
- **Primitive fitting (approximation)** — fit a `Part::Plane`, `Part::Cylinder`,
  `Part::Sphere`, Bézier surface, or B-spline surface to a mesh segment or
  point cloud.
- **Boundary extraction** — extract the boundary loops of a mesh as `Part`
  wires or faces, which can then be used as profiles for Part Design features.

---

## Typical reverse-engineering workflow

```
3-D scan
   │
   ▼
Import scan (Points workbench)       Import mesh (Mesh workbench)
   │                                       │
   ▼                                       ▼
Poisson Reconstruction          Mesh Segmentation / Manual Segmentation
   │                                       │
   ▼                                       ▼
mesh                             mesh segments (per surface type)
   │                                       │
   └──────────────────┬───────────────────┘
                      ▼
            Approximation tools
            (Plane / Cylinder / Sphere /
             B-Spline Surface / B-Spline Curve)
                      │
                      ▼
               Part geometry (solid, face, edge)
                      │
                      ▼
            Part Design / Sketcher for further refinement
```

---

## Menu structure

The workbench adds a **Reverse Engineering** menu with three submenus:

| Submenu | Tools |
|---------|-------|
| **Surface Reconstruction** | Poisson…, Structured Point Clouds |
| **Segmentation** | Mesh Segmentation…, Manual Segmentation…, From Components, Wire From Mesh Boundary…; also Remesh with Gmsh, Vertex Curvature, Curvature Info (from Mesh workbench) |
| **Approximation** | Plane, Cylinder, Sphere, Polynomial Surface, Approximate B-Spline Surface…, Approximate B-Spline Curve… |

---

## Input data requirements

| Tool group | Accepted input |
|------------|---------------|
| Surface Reconstruction | Point cloud (`Points::Feature` with normals) |
| Structured Point Clouds | Structured point cloud (`Points::Structured`) |
| Segmentation | Mesh (`Mesh::Feature`) |
| Wire From Mesh Boundary | Mesh (`Mesh::Feature`) |
| Approximation — Plane | Any geometry |
| Approximation — Cylinder / Sphere / Polynomial | Mesh (`Mesh::Feature`) |
| Approximation — B-Spline Surface | Point cloud or Mesh |
| Approximation — B-Spline Curve | Point cloud |

---

## See also

- [Poisson Reconstruction](tools/poisson-reconstruction.md) — watertight mesh from point cloud
- [Mesh Segmentation](tools/mesh-segmentation.md) — auto-split mesh by surface type
- [Approximate B-Spline Surface](tools/approx-bspline-surface.md) — fit free-form NURBS
- [All Tools](tools/index.md) — complete tool reference
- Points workbench — import and manage point clouds
- Mesh workbench — mesh repair before reverse engineering
- Surface workbench — free-form surface tools for the resulting geometry
