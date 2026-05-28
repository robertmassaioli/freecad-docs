# Mesh Tools

Complete index of all Mesh workbench tools.

---

## Import and Export

| Tool | Description |
|------|-------------|
| [Import Mesh](import-export.md#import-mesh) | Load a mesh from an STL, OBJ, OFF, PLY, or AST file |
| [Export Mesh](import-export.md#export-mesh) | Save the selected mesh to a file |
| [From Part Shape](import-export.md#from-part-shape) | Tessellate a solid into a mesh |
| [Remesh with Gmsh](import-export.md#remesh-with-gmsh) | Re-mesh an existing mesh using Gmsh |
| [Build Regular Solid](import-export.md#build-regular-solid) | Create a mesh primitive (cube, sphere, cylinder, etc.) |

---

## Analyse

| Tool | Description |
|------|-------------|
| [Evaluate and Repair](analyse.md#evaluate-and-repair) | Comprehensive mesh quality check and repair dialog |
| [Evaluate Facet](analyse.md#evaluate-facet) | Inspect the properties of a single triangle |
| [Curvature Info](analyse.md#curvature-info) | Report curvature values at a picked point |
| [Evaluate Solid](analyse.md#evaluate-solid) | Check if the mesh is a closed solid |
| [Bounding Box](analyse.md#bounding-box) | Display the mesh bounding box dimensions |
| [Vertex Curvature](analyse.md#vertex-curvature) | Display a colour map of vertex curvature |

---

## Modify

| Tool | Description |
|------|-------------|
| [Harmonize Normals](modify.md#harmonize-normals) | Make all triangle normals point consistently outward |
| [Flip Normals](modify.md#flip-normals) | Reverse all triangle normals |
| [Fill Up Holes](modify.md#fill-up-holes) | Automatically fill all open boundary holes |
| [Fill Hole Interactively](modify.md#fill-hole-interactively) | Pick and fill individual holes |
| [Add Facet](modify.md#add-facet) | Add a single triangle to the mesh |
| [Remove Components](modify.md#remove-components) | Delete disconnected mesh components below a size threshold |
| [Remove Component by Hand](modify.md#remove-component-by-hand) | Interactively select and delete mesh regions |
| [Smoothing](modify.md#smoothing) | Smooth the mesh surface (Laplacian or Taubin) |
| [Decimating](modify.md#decimating) | Reduce the triangle count while preserving shape |
| [Scale](modify.md#scale) | Scale the mesh by a factor |

---

## Boolean Operations

| Tool | Description |
|------|-------------|
| [Union](boolean-cutting.md#union) | Boolean union of two meshes |
| [Intersection](boolean-cutting.md#intersection) | Boolean intersection of two meshes |
| [Difference](boolean-cutting.md#difference) | Boolean difference of two meshes |

---

## Cutting

| Tool | Description |
|------|-------------|
| [Cut by Polygon](boolean-cutting.md#cut-by-polygon) | Cut the mesh with a screen-drawn polygon |
| [Trim by Polygon](boolean-cutting.md#trim-by-polygon) | Trim the mesh to a polygon boundary |
| [Trim by Plane](boolean-cutting.md#trim-by-plane) | Trim the mesh at a plane |
| [Section by Plane](boolean-cutting.md#section-by-plane) | Create a cross-section wire at a plane |
| [Cross-Sections](boolean-cutting.md#cross-sections) | Create multiple parallel cross-section wires |

---

## Segmentation

| Tool | Description |
|------|-------------|
| [Merge](segmentation.md#merge) | Merge two or more meshes into one |
| [Split Components](segmentation.md#split-components) | Split a mesh into its disconnected components |
| [Segmentation](segmentation.md#segmentation-curvature) | Segment the mesh by curvature into regions |
| [Segmentation — Best Fit](segmentation.md#segmentation-best-fit) | Segment by fitting geometric primitives |
