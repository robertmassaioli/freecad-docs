# Points Workbench

> **In one sentence:** The Points workbench imports, exports, and manipulates
> 3-D point clouds — unordered or structured grid — with optional per-point
> colour, normal, and intensity data, and bridges point cloud data into other
> FreeCAD workbenches such as Mesh and Inspection.

---

## What is the Points workbench for?

A **point cloud** is a set of X, Y, Z coordinates sampled from the surface of
a real object — typically produced by a 3-D laser scanner, structured-light
scanner, or photogrammetry pipeline. Unlike a mesh or solid, a point cloud
carries no connectivity: each point is independent.

The Points workbench provides tools to:

- **Import** point clouds from scanner output files (.asc, .pcd, .ply, .e57).
- **Export** them to standard formats.
- **Convert** any FreeCAD geometry (mesh, solid) into a point cloud for
  downstream analysis.
- **Restructure** an unordered scan into a width × height grid (structured
  cloud) for sensors that produce ordered images.
- **Cut** a cloud interactively with a lasso to remove unwanted regions.
- **Merge** multiple clouds into one.

Point clouds created in the Points workbench can be used directly as **Actual**
geometry in the Inspection workbench.

---

## Point cloud types

| Type | C++ class | Description |
|------|-----------|-------------|
| **Unstructured** | `Points::Feature` | An unsorted list of X, Y, Z coordinates. This is the default after import. |
| **Structured** | `Points::Structured` | An ordered Width × Height grid of points. Invalid (missing) scan positions are represented by `NaN`. |

Both types share the same `Points` property (a `PropertyPointKernel`).
`Structured` additionally stores `Width` and `Height` integers.

---

## Optional per-point attributes

Scanners often produce additional data alongside the X, Y, Z coordinates.
FreeCAD stores these as optional extra properties:

| Property | Class | Display mode |
|----------|-------|-------------|
| `Color` | `App::PropertyColorList` | **Color** — points coloured by their RGB value |
| `Normal` | `Points::PropertyNormalList` | **Shaded** — shading computed from per-point normals |
| `Intensity` | `Points::PropertyGreyValueList` | **Intensity** — greyscale based on scanner return intensity |

The **Display Mode** setting in the View tab of the Properties panel selects
which attribute is used for rendering. The default "Points" mode renders all
points in a uniform colour.

---

## File format support

| Format | Extension | Import | Export | Notes |
|--------|-----------|--------|--------|-------|
| ASCII point list | `.asc` | ✓ | ✓ | Plain text X Y Z (one point per line), optional extra columns |
| PCD (Point Cloud Data) | `.pcd` | ✓ | ✓ | PCL-compatible binary or ASCII; supports fields, normals |
| PLY (Polygon File Format) | `.ply` | ✓ | ✓ | Supports colour and normal attributes |
| E57 | `.e57` | ✓ | — | ASTM standard for 3-D imaging data, common from industrial scanners |

---

## Typical workflow

```
3-D scan output file
       │
       ▼
  Import Points  ──────────────────────────────────────────────────►  Points::Feature
                                                                              │
         ┌────────────────────────────────────────────────────────────────────┤
         │                                                                    │
         ▼                                                                    ▼
  Cut Point Cloud                                                  Structured Point Cloud
  (remove noise, clipping)                                        (order for sensor grid)
         │                                                                    │
         ▼                                                                    ▼
  Merge Point Clouds ──────────────────────────────────────────────►  merged cloud
         │
         ▼
  Inspection workbench (as Actual) or
  Mesh workbench (Convert to Points → reverse engineering)
         │
         ▼
  Export Points
```

---

## See also

- [Points Tools](tools/index.md) — all tools
- Inspection workbench — compare a point cloud against a design model
- Mesh workbench — mesh reconstruction from point clouds (via external tools)
- ReverseEngineering workbench — surface fitting from point clouds
