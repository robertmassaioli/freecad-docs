# Points from Shape

> **In one sentence:** Extract all vertices from a Part shape and create a
> Points workbench object containing them as a point cloud.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Points from Shape  
**Shortcut:** none

Extracts all vertex coordinates from a selected Part shape and creates a
`Points::Feature` object in the current document containing those points.

---

## Intuition

B-rep shapes are defined by faces, edges, and vertices. Points from Shape goes
in the opposite direction of [Shape from Mesh](shape-from-mesh.md): it takes
the discrete vertex coordinates out of the B-rep geometry and gives you a
cloud of points.

This is useful when downstream tools need raw coordinates (scripts, spreadsheets,
point-cloud-based analysis) rather than the full topological B-rep.

---

## When to use it

- You need the corner coordinates of a solid as input for a script or
  spreadsheet.
- You want to visualise the vertex positions of a complex shape.
- You are feeding vertex data into a point-cloud or reverse-engineering tool.
- You need to export key geometry positions as an ASC or PCD file.

## When NOT to use it

- **Don't use it if you need continuous surface data** — vertices give you
  corner points only, not points along curved edges or faces. For sampling
  points along surfaces, use a script that samples edge/face parametric
  coordinates.
- **Don't use it as a way to convert a Part solid to a mesh** — use the Mesh
  workbench export or **Mesh → Create Mesh from Shape** instead.

---

## Step-by-step

1. Select the Part shape in the model tree or 3D view.
2. Choose **Part → Points from Shape**.
3. A `Points::Feature` object appears in the tree containing the extracted
   vertex coordinates.

---

## Parameters

This tool has no task panel. All vertices of the selected shape are extracted
automatically.

---

## Common mistakes and pitfalls

!!! warning "Only vertices are extracted, not edge or face sample points"
    The resulting point cloud contains only the discrete vertex positions —
    endpoints of edges and corners of faces. Curved edges contribute only
    their two endpoints; faces contribute only their corner vertices. The
    output is not a dense sampling of the shape surface.

!!! warning "The result is not parametric"
    The extracted point cloud is a static snapshot. If the original shape
    changes, the point cloud is not updated. Re-run the tool if needed.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()
box = doc.addObject("Part::Box", "Box")
box.Length = 20; box.Width = 15; box.Height = 10
doc.recompute()

# Access vertex coordinates directly from the shape
for i, vertex in enumerate(box.Shape.Vertexes):
    print(f"Vertex {i}: {vertex.Point}")

# The interactive tool creates a Points::Feature object;
# for scripting, you can use the Points module:
import Points
pts = Points.Points()
pts.addPoints([v.Point for v in box.Shape.Vertexes])
feat = doc.addObject("Points::Feature", "VertexCloud")
feat.Points = pts
doc.recompute()
print(f"Points extracted: {feat.Points.Count}")
```

---

## See also

- [Shape from Mesh](shape-from-mesh.md) — convert a mesh to a Part shape (reverse direction)
- [Convert to Solid](convert-to-solid.md) — close a shell into a solid
- Points workbench — import, export, and process point clouds
- [Part Workbench](../index.md) — workbench overview
