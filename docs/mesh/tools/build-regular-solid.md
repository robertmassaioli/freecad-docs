# Build Regular Solid

> **In one sentence:** Create a mesh primitive (cube, cylinder, sphere,
> cone, ellipsoid, or torus) directly in the Mesh workbench without
> needing a B-rep solid first.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Build Solid…  
**Command:** `Mesh_BuildRegularSolid`  
**Shortcut:** none

Opens the Regular Solid dialog to create a mesh primitive directly as a
`Mesh::Feature`, without going through the Part workbench.

---

## Intuition

The mesh primitives here are triangulated approximations of the same shapes
available in Part workbench (cube, sphere, cylinder, etc.). They are useful
when you need a quick mesh reference shape for boolean operations or visual
comparisons in the Mesh workbench, without the overhead of creating a parametric
B-rep solid first.

For engineering use, the Part workbench primitives are always preferable — they
are exact, not approximate. Use Build Regular Solid only for mesh-native
workflows.

---

## When to use it

- You need a quick mesh reference shape for a mesh boolean operation.
- You are testing mesh operations and need a clean primitive mesh.
- You want a mesh sphere or torus without creating a B-rep solid first.

## When NOT to use it

- **Don't use it for engineering models** — the meshes are approximate
  triangulations. Use [Part primitives](../../part/tools/index.md) for exact
  geometry, then [From Part Shape](from-part-shape.md) to convert.

---

## Available primitives

| Primitive | Parameters |
|-----------|-----------|
| Cube | Length, Width, Height |
| Cylinder | Radius, Length, Closed, Edge angle, Number of edges |
| Cone | Radius 1, Radius 2, Length, Closed, Edge angle, Edges |
| Sphere | Radius, Edge angle, Number of edges (lat/lon) |
| Ellipsoid | Radius 1, Radius 2, Edge angle, Edges |
| Torus | Radius 1, Radius 2, Edge angle, Edges |

---

## Common mistakes and pitfalls

!!! warning "These are mesh approximations, not exact geometry"
    The sphere, cylinder, and torus are tessellated approximations. Their
    dimensions are nominal — the actual surface deviates from the ideal
    surface by an amount determined by the number of edges (facets).
    Increase the edge count for a smoother approximation.

---

## See also

- [From Part Shape](from-part-shape.md) — exact-to-mesh conversion for engineering work
- [Import Mesh](import-mesh.md) — import existing mesh files
- [Mesh Workbench](../index.md) — workbench overview
