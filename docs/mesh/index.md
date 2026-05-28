# Mesh Workbench

> **In one sentence:** The Mesh workbench works with triangulated surface
> meshes — importing and exporting STL/OBJ/OFF files, analysing mesh
> quality, repairing defects, applying boolean operations, cutting meshes
> with planes or polygons, and converting between meshes and parametric
> solids.

---

## What is a mesh?

A **mesh** is a collection of triangular facets that approximate a surface.
Each triangle is defined by three vertices and a normal direction. Meshes
are the simplest and most portable geometry format — used in 3-D printing
(STL), game assets (OBJ), and scan data (PLY, OFF).

**Meshes vs B-rep solids:**

| Property | Mesh | B-rep solid (Part/Part Design) |
|----------|------|-------------------------------|
| Geometry | Triangles | Exact surfaces (NURBS, planes, spheres) |
| Accuracy | Approximate (depends on mesh resolution) | Exact to floating-point precision |
| File size | Can be very large (millions of triangles) | Compact (topological description) |
| Editing | Vertex, edge, facet level | Feature-level (pad, pocket, fillet) |
| Primary use | 3-D printing, scan data, game assets | Engineering design, manufacturing |

---

## What is the Mesh workbench for?

The Mesh workbench serves several use cases:

1. **Importing scan data** (photogrammetry, structured light, CT scans)
   in STL, OBJ, or PLY format for reference or reverse engineering.

2. **Preparing STL for 3-D printing** — analyzing and repairing
   non-manifold geometry, filling holes, harmonizing normals.

3. **Converting solids to meshes** — generating a triangulated mesh
   from a Part Design solid for export to STL.

4. **Working with mesh geometry** — boolean operations, cutting,
   smoothing, decimating, segmenting.

5. **Unffolding flat meshes** — the optional MeshPart Flatten tools
   produce flat layouts from curved mesh surfaces (requires flatmesh).

---

## Key concepts

### Manifold geometry

A **manifold mesh** is a watertight, connected surface where every edge
is shared by exactly two triangles and no triangle intersects another.
Non-manifold defects cause problems:

| Defect | Description | Effect on 3-D printing |
|--------|-------------|----------------------|
| Open boundary | Edge with only one adjacent triangle | Holes in the surface |
| Non-manifold edge | Edge with 3+ adjacent triangles | Ambiguous inside/outside |
| Self-intersection | Triangles penetrate each other | Slicer errors |
| Inconsistent normals | Mixed inward/outward facing triangles | Inside-out geometry |
| Duplicate vertices | Two points at the same position | Disconnected facets |

### Inside vs outside (normal direction)

Every triangle has a normal that points "outward". For a closed solid,
all normals must point away from the enclosed volume. If some normals
point inward, the mesh is "inside out" in places — **Harmonize Normals**
and **Flip Normals** fix this.

---

## Typical workflows

### STL → FreeCAD solid

```
1. Mesh → Import (load STL)
2. Analyze → Evaluation (check for defects)
3. Fix defects (Fill holes, Remove components, Harmonize normals)
4. Part workbench → Part → Convert to Solid  (or use ReverseEngineering)
```

### Part Design solid → STL for 3-D printing

```
1. Design the part in Part Design workbench
2. Mesh workbench → Meshes → From Part Shape
3. Set mesh resolution (Max size = 0.5–1.0 mm for typical prints)
4. Meshes → Export → STL
```

---

## See also

- [Import and Export](tools/import-export.md) — file formats and solid conversion
- [Analyse](tools/analyse.md) — mesh quality evaluation
- [Modify](tools/modify.md) — repair, smooth, decimate
- [Boolean and Cutting](tools/boolean-cutting.md) — boolean ops and cross-sections
- [Segmentation](tools/segmentation.md) — split and merge meshes
