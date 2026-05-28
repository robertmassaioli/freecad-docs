# Analyse

> **In one sentence:** The Analyse tools evaluate mesh quality —
> checking for defects, measuring curvature, verifying solid closure,
> and reporting bounding box dimensions.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Analyze (submenu)

| Tool | Description |
|------|-------------|
| [Evaluate and Repair](#evaluate-and-repair) | Comprehensive defect check and repair dialog |
| [Evaluate Facet](#evaluate-facet) | Inspect one triangle |
| [Curvature Info](#curvature-info) | Report curvature at a picked point |
| [Evaluate Solid](#evaluate-solid) | Check if the mesh is a closed solid |
| [Bounding Box](#bounding-box) | Display the mesh bounding box |
| [Vertex Curvature](#vertex-curvature) | Colour map of curvature across the mesh |

---

## Intuition

### Why analyse a mesh?

Every STL file from external tools, photogrammetry software, or CT scan
processing should be analysed before further use. Common defects:

| Defect | Cause | Effect |
|--------|-------|--------|
| Open boundaries (holes) | Incomplete scan, export errors | Non-printable, slicer errors |
| Non-manifold edges | Duplicate triangles, T-junctions | Invalid geometry |
| Self-intersections | Poor reconstruction, bad booleans | Incorrect prints |
| Inconsistent normals | Mixed inward/outward facing | Inside-out slices |
| Degenerate triangles | Zero-area triangles | Solver/slicer crashes |
| Duplicate points | Adjacent triangles not connected | Disconnected surface |

### Curvature as a quality metric

Curvature measures how sharply a surface bends. Mesh curvature is
computed per vertex and approximated from the angles of surrounding
triangles. High curvature appears at edges, corners, and sharp features.

---

## Evaluate and Repair

**Menu:** Mesh → Meshes → Analyze → Evaluate & Repair  
**Command:** `Mesh_Evaluation`

Opens the Evaluate & Repair dialog — the most comprehensive mesh analysis
tool. It checks for multiple defect types and offers one-click repair
for each.

### Checks performed

| Check | What it finds | Repair action |
|-------|--------------|---------------|
| Points duplicates | Vertices at the same or nearly the same position | Merge nearby points |
| Face indices | Invalid triangle index references | Remove invalid faces |
| Holes | Open boundary edges (unshared edges) | Fill all holes automatically |
| Non-manifold points | Vertices connected to a non-manifold configuration | Remove non-manifold points |
| Non-manifold edges | Edges shared by more than 2 triangles | Remove non-manifold edges |
| Orientations | Inconsistent triangle winding | Harmonize normals |
| Normals | Outward-pointing vs inward-pointing | Flip normals |
| Self-intersections | Triangles that penetrate each other | — (no automatic fix) |
| Degenerate faces | Zero-area triangles | Remove degenerate faces |

### Recommended workflow for imported STL

1. Open **Evaluate & Repair**.
2. Click **Analyze** (all checks).
3. Review the report — green = OK, red = defects found.
4. For each defect, click **Repair** to apply the automatic fix.
5. Re-run **Analyze** to confirm all issues are resolved.
6. Close the dialog.

### Self-intersection caveat

Self-intersections cannot be automatically repaired — the geometry is
ambiguous. Fix by going back to the source CAD tool and re-exporting,
or using manual triangle editing.

---

## Evaluate Facet

**Menu:** Mesh → Meshes → Analyze → Evaluate Facet  
**Command:** `Mesh_EvaluateFacet`

Activates a pick mode — click on any triangle in the mesh to display its
properties in the Task panel:

| Property | Description |
|----------|-------------|
| Face index | Triangle index in the mesh data structure |
| Triangle area | Area of the triangle (mm²) |
| Normal | X, Y, Z components of the outward normal vector |
| Aspect ratio | Longest edge / shortest altitude (quality metric) |
| Neighbours | Indices of adjacent triangles |
| Neighbours' normals | Normals of adjacent triangles |

**Use cases:**
- Finding degenerate triangles with near-zero area.
- Checking the normal direction at a specific location.
- Investigating a region that looks wrong in the colour-map view.

---

## Curvature Info

**Menu:** Mesh → Meshes → Analyze → Curvature Info  
**Command:** `Mesh_CurvatureInfo`

Activates a pick mode — click on the mesh to report the curvature at the
nearest vertex:

| Value | Description |
|-------|-------------|
| Min curvature | Principal curvature κ₁ (lower value) |
| Max curvature | Principal curvature κ₂ (higher value) |
| Mean curvature | (κ₁ + κ₂) / 2 |
| Gaussian curvature | κ₁ × κ₂ |

For a flat face: both principal curvatures = 0.  
For a cylinder wall: κ₁ = 1/R (hoop direction), κ₂ = 0 (axial direction).  
For a sphere: κ₁ = κ₂ = 1/R.

This tool requires a Vertex Curvature object to already exist (run
**Vertex Curvature** first to compute the curvature data).

---

## Evaluate Solid

**Menu:** Mesh → Meshes → Analyze → Check Solid  
**Command:** `Mesh_EvaluateSolid`

Checks whether the selected mesh is a **closed, manifold solid** — a
requirement for watertight 3-D printing and valid boolean operations.

The result is shown in a dialog:
- **Solid**: The mesh is closed and manifold. All edges are shared by
  exactly two triangles.
- **Not solid**: The mesh has open boundaries. Show the holes with
  **Evaluate and Repair** to find and fix them.

---

## Bounding Box

**Menu:** Mesh → Meshes → Analyze → Boundings info  
**Command:** `Mesh_BoundingBox`

Displays the axis-aligned bounding box of the selected mesh:

| Value | Description |
|-------|-------------|
| X min / X max | Extent in the X direction |
| Y min / Y max | Extent in the Y direction |
| Z min / Z max | Extent in the Z direction |
| Width (X) | X max − X min |
| Depth (Y) | Y max − Y min |
| Height (Z) | Z max − Z min |

Use this to check that imported meshes are at the correct scale (e.g.
a human-sized part should be ~200 mm tall, not 200 inches).

---

## Vertex Curvature

**Menu:** Mesh → Meshes → Vertex Curvature  
**Command:** `Mesh_VertexCurvature`

Creates a **Curvature** object in the model tree that stores curvature
values at every vertex of the mesh. The mesh is displayed with a colour
map showing curvature variation:

| Colour | Curvature meaning |
|--------|------------------|
| Blue | Low curvature (flat regions) |
| Green | Moderate curvature |
| Red | High curvature (sharp edges, corners) |

### Reading the colour map

- Flat faces appear uniformly blue.
- Curved surfaces (cylinders, spheres) show a gradient.
- Sharp edges and corners appear red.
- Isolated red dots may indicate degenerate triangles or non-manifold points.

### Using with Curvature Info

After computing Vertex Curvature, activate **Curvature Info** to click
individual vertices and read their exact curvature values.

### Python API

```python
import Mesh

mesh = App.ActiveDocument.getObject("Mesh")
curvature = App.ActiveDocument.addObject("Mesh::Curvature", "MeshCurvature")
curvature.Source = mesh
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Curvature Info requires Vertex Curvature to exist first"
    If you try to use Curvature Info without first running Vertex Curvature,
    no curvature data is available and the tool will not report values.
    Always compute Vertex Curvature before using Curvature Info.

!!! warning "Self-intersections require manual repair"
    Evaluate & Repair detects self-intersections but cannot automatically
    fix them. The geometry is ambiguous — only the original designer knows
    which triangle should take precedence. Fix at the source tool.

!!! warning "A mesh can be solid but still have self-intersections"
    Evaluate Solid only checks for open boundaries. A mesh can pass the
    solid check while still having self-intersections that will cause
    slicer errors. Always run the full **Evaluate & Repair** check.

---

## See also

- [Modify](modify.md) — repair tools for defects found during analysis
- [Import and Export](import-export.md) — import meshes for analysis
