# Boolean Operations and Cutting

> **In one sentence:** Boolean tools combine or subtract meshes
> (union, intersection, difference), while cutting tools trim meshes
> with polygons or planes and create cross-section wires.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Boolean / Cutting (submenus)

### Boolean operations

| Tool | Description |
|------|-------------|
| [Union](#union) | Boolean union — combine two meshes into one |
| [Intersection](#intersection) | Boolean intersection — keep only the overlap |
| [Difference](#difference) | Boolean difference — subtract one mesh from another |

### Cutting tools

| Tool | Description |
|------|-------------|
| [Cut by Polygon](#cut-by-polygon) | Cut the mesh with a screen-drawn polygon |
| [Trim by Polygon](#trim-by-polygon) | Trim keeping one side of a polygon cut |
| [Trim by Plane](#trim-by-plane) | Trim the mesh at a plane |
| [Section by Plane](#section-by-plane) | Create a cross-section wire at a plane |
| [Cross-Sections](#cross-sections) | Create multiple parallel cross-section wires |

---

## Intuition

### Mesh booleans vs B-rep booleans

| Aspect | Mesh boolean | B-rep boolean (Part workbench) |
|--------|-------------|-------------------------------|
| Input | Triangle meshes | Exact B-rep solids |
| Result | Triangle mesh | Exact B-rep solid |
| Accuracy | Approximate (mesh resolution-dependent) | Exact |
| Speed | Fast for coarse meshes | Slower but exact |
| Reliability | Fails on poor meshes | Fails on degenerate topology |
| Best for | Scan data, STL files | Engineering design |

Mesh booleans require **solid** meshes (no open boundaries, manifold).
Always run **Evaluate & Repair** before applying boolean operations.

### Cutting vs trimming

- **Cut** — removes part of the mesh, leaving an open boundary where
  the cut was made.
- **Trim** — removes part of the mesh AND closes the opening with a flat
  cap (if the cut is planar).

---

## Union

**Menu:** Mesh → Meshes → Boolean → Union  
**Command:** `Mesh_Union`

Computes the boolean union of two selected meshes — the resulting mesh
encloses the volume of both inputs. Overlapping regions are merged into
a single continuous surface.

### Step-by-step

1. Select the first mesh in the model tree.
2. Hold Ctrl and select the second mesh.
3. Choose **Mesh → Meshes → Boolean → Union**.
4. A new mesh object appears containing the union.
5. The original meshes are retained (not deleted).

### Requirements

- Both meshes must be solid (watertight, manifold).
- The meshes may or may not overlap — non-overlapping solids produce a
  compound mesh, not a true union.

### Python API

```python
import Mesh

m1 = App.ActiveDocument.getObject("Mesh")
m2 = App.ActiveDocument.getObject("Mesh001")

result = m1.Mesh.copy()
result.unite(m2.Mesh)

feature = App.ActiveDocument.addObject("Mesh::Feature", "Union")
feature.Mesh = result
App.ActiveDocument.recompute()
```

---

## Intersection

**Menu:** Mesh → Meshes → Boolean → Intersection  
**Command:** `Mesh_Intersection`

Computes the boolean intersection — the mesh that represents **only** the
volume where both input meshes overlap. If the meshes do not overlap, the
result is empty.

### Use cases

- Finding the common volume between two overlapping scan meshes.
- Computing the intersection of a tool path solid with a part.

---

## Difference

**Menu:** Mesh → Meshes → Boolean → Difference  
**Command:** `Mesh_Difference`

Computes the boolean difference — the first mesh minus the second mesh.
The volume occupied by the second mesh is removed from the first.

**Order matters:** Difference is not commutative.
- Selecting mesh A then B → A minus B.
- Selecting mesh B then A → B minus A.

### Use cases

- Cutting a slot or hole through a solid mesh.
- Subtracting a tool shape from a workpiece mesh.

---

## Cut by Polygon

**Menu:** Mesh → Meshes → Cutting → Cut by Polygon  
**Command:** `Mesh_PolyCut`

Cuts the mesh by drawing a closed polygon on the screen (in 2-D view
space). The polygon is projected onto the mesh and used as a cutting
boundary. Mesh elements on one side of the polygon are removed.

### Step-by-step

1. Orient the 3-D view from the direction you want to cut (use standard
   views: Top, Front, etc.).
2. Activate **Mesh → Meshes → Cutting → Cut by Polygon**.
3. Left-click to add vertices of the cutting polygon.
4. Right-click to close the polygon and perform the cut.
5. FreeCAD prompts: **Inner** (keep inside the polygon) or **Outer**
   (keep outside). Click the desired side.

### Notes

- The cut is made in 2-D screen space — it is a vertical projection
  through the view direction.
- Elements that are only partially inside the polygon are trimmed at the
  polygon boundary.
- The resulting mesh has an open boundary at the cut.

---

## Trim by Polygon

**Menu:** Mesh → Meshes → Cutting → Trim by Polygon  
**Command:** `Mesh_PolyTrim`

Similar to Cut by Polygon, but:
- Removes the mesh elements outside the drawn polygon.
- The remaining mesh has the polygon outline as its boundary.

This is equivalent to Cut by Polygon choosing "Inner" — keep only what
is inside the screen polygon.

---

## Trim by Plane

**Menu:** Mesh → Meshes → Cutting → Trim by Plane  
**Command:** `Mesh_TrimByPlane`

Trims the mesh by a plane defined interactively in the 3-D view. The mesh
is cut at the plane and one half is discarded.

### Step-by-step

1. Activate **Mesh → Meshes → Cutting → Trim by Plane**.
2. Pick three points in the 3-D view to define the cutting plane.
3. FreeCAD shows the plane and asks which side to keep.
4. Click **Above** or **Below** to remove one side.

The result is an open mesh with the cut boundary visible.

---

## Section by Plane

**Menu:** Mesh → Meshes → Cutting → Section by Plane  
**Command:** `Mesh_SectionByPlane`

Creates a **cross-section wire** at the intersection of a plane with the
mesh. The result is a set of line segments (a `Part::Feature` wire) at
the cut plane, not a trimmed mesh.

### Use cases

- Extracting a 2-D cross-section profile for further use in Draft or
  Sketcher.
- Measuring the profile of an imported scan at a specific height.
- Creating a contour for CNC machining reference.

### Step-by-step

1. Select the mesh.
2. Activate **Mesh → Meshes → Cutting → Section by Plane**.
3. Pick three points to define the cutting plane, or use the task panel
   to set plane parameters (point + normal).
4. A wire appears at the intersection.

---

## Cross-Sections

**Menu:** Mesh → Meshes → Cutting → Cross-Sections  
**Command:** `Mesh_CrossSections`

Creates **multiple parallel section wires** through the mesh at regular
intervals. This is the batch version of **Section by Plane**.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Plane | XY, XZ, or YZ — the orientation of the cutting planes |
| Position (start) | First cutting plane position along the normal axis |
| Position (end) | Last cutting plane position |
| Count | Number of sections between start and end |
| Connections | Connect the section contours with line segments |

### Use cases

- Generating contour maps of a terrain mesh.
- Creating layer-by-layer profiles for large format printing.
- Inspecting the internal shape of a closed mesh at multiple heights.

---

## Common mistakes and pitfalls

!!! warning "Mesh booleans require watertight meshes"
    Boolean operations fail silently or produce corrupt results if either
    input mesh has holes, non-manifold edges, or self-intersections.
    Run **Analyse → Evaluate & Repair** on both meshes before attempting
    booleans.

!!! warning "Cut by Polygon depends on view orientation"
    The cut is performed in 2-D screen space. If the view is perspective
    (not orthographic) or the viewpoint is not perfectly aligned, the cut
    plane will be oblique. Use standard views (Top/Front/Side) and switch
    to orthographic projection (**View → Standard views → Orthographic**).

!!! warning "Section by Plane produces a wire, not a solid"
    The result of Section by Plane and Cross-Sections is a set of edges,
    not a face or solid. To use the section as a sketch profile, switch
    to Sketcher and import the wire as external geometry.

!!! warning "Union of non-overlapping meshes is not a single solid"
    If two meshes do not intersect, their union is a compound of two
    disconnected meshes — not a single fused solid. They appear as one
    object in the tree but are not topologically connected.

---

## See also

- [Analyse](analyse.md) — verify meshes are solid before booleans
- [Modify](modify.md) — repair meshes after boolean operations
- [Segmentation](segmentation.md) — split mesh components after cutting
