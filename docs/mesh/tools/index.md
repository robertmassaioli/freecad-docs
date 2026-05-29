# Mesh Tools

Complete index of all Mesh workbench tools.

---

## Import and Export

| Tool | Description |
|------|-------------|
| [Import Mesh](import-mesh.md) | Load a mesh from an STL, OBJ, OFF, PLY, or AST file |
| [Export Mesh](export-mesh.md) | Save the selected mesh to a file |
| [From Part Shape](from-part-shape.md) | Tessellate a solid into a mesh |
| [Remesh with Gmsh](remesh-with-gmsh.md) | Re-mesh an existing mesh using Gmsh |
| [Build Regular Solid](build-regular-solid.md) | Create a mesh primitive (cube, sphere, cylinder, etc.) |

---

## Analyse

| Tool | Description |
|------|-------------|
| [Evaluate and Repair](evaluate-and-repair.md) | Comprehensive mesh quality check and repair dialog |
| [Evaluate Facet](evaluate-facet.md) | Inspect the properties of a single triangle |
| [Curvature Info](curvature-info.md) | Report curvature values at a picked point |
| [Evaluate Solid](evaluate-solid.md) | Check if the mesh is a closed solid |
| [Bounding Box](bounding-box.md) | Display the mesh bounding box dimensions |
| [Vertex Curvature](vertex-curvature.md) | Display a colour map of vertex curvature |

---

## Modify

| Tool | Description |
|------|-------------|
| [Harmonize Normals](harmonize-normals.md) | Make all triangle normals point consistently outward |
| [Flip Normals](flip-normals.md) | Reverse all triangle normals |
| [Fill Up Holes](fill-up-holes.md) | Automatically fill all open boundary holes |
| [Fill Hole Interactively](fill-hole-interactively.md) | Pick and fill individual holes |
| [Add Facet](add-facet.md) | Add a single triangle to the mesh |
| [Remove Components](remove-components.md) | Delete disconnected mesh components below a size threshold |
| [Remove Component by Hand](remove-component-by-hand.md) | Interactively select and delete mesh regions |
| [Smoothing](smoothing.md) | Smooth the mesh surface (Laplacian or Taubin) |
| [Decimating](decimating.md) | Reduce the triangle count while preserving shape |
| [Scale](scale.md) | Scale the mesh by a factor |

---

## Boolean Operations

| Tool | Description |
|------|-------------|
| [Union](mesh-union.md) | Boolean union of two meshes |
| [Intersection](mesh-intersection.md) | Boolean intersection of two meshes |
| [Difference](mesh-difference.md) | Boolean difference of two meshes |

---

## Cutting

| Tool | Description |
|------|-------------|
| [Cut by Polygon](cut-by-polygon.md) | Cut the mesh with a screen-drawn polygon |
| [Trim by Polygon](trim-by-polygon.md) | Trim the mesh to a polygon boundary |
| [Trim by Plane](trim-by-plane.md) | Trim the mesh at a plane |
| [Section by Plane](section-by-plane.md) | Create a cross-section wire at a plane |
| [Cross-Sections](cross-sections.md) | Create multiple parallel cross-section wires |

---

## Segmentation

| Tool | Description |
|------|-------------|
| [Merge](merge.md) | Merge two or more meshes into one |
| [Split Components](split-components.md) | Split a mesh into its disconnected components |
| [Segmentation](segmentation-curvature.md) | Segment the mesh by curvature into regions |
| [Segmentation — Best Fit](segmentation-best-fit.md) | Segment by fitting geometric primitives |
