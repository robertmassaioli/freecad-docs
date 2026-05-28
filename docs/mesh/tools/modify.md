# Modify

> **In one sentence:** The Modify tools repair, clean, and simplify
> meshes — fixing normals, filling holes, removing fragments, smoothing
> rough surfaces, reducing triangle count, rescaling, and re-meshing.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes

| Tool | Description |
|------|-------------|
| [Harmonize Normals](#harmonize-normals) | Make all normals consistently outward |
| [Flip Normals](#flip-normals) | Reverse all triangle normals |
| [Fill Up Holes](#fill-up-holes) | Automatically fill all open boundary holes |
| [Fill Hole Interactively](#fill-hole-interactively) | Pick and fill individual holes |
| [Add Facet](#add-facet) | Add a single triangle manually |
| [Remove Components](#remove-components) | Delete disconnected components below a size threshold |
| [Remove Component by Hand](#remove-component-by-hand) | Interactively select and delete regions |
| [Smoothing](#smoothing) | Smooth the surface using Laplacian or Taubin filter |
| [Decimating](#decimating) | Reduce triangle count while preserving shape |
| [Scale](#scale) | Rescale the mesh by a factor |

---

## Harmonize Normals

**Menu:** Mesh → Meshes → Harmonize Normals  
**Command:** `Mesh_HarmonizeNormals`

Analyses the connectivity of the mesh and propagates a consistent normal
direction across all triangles. The algorithm picks a seed triangle and
flips adjacent triangles whose normals disagree with it by more than 90°.

### When to use

- After importing a mesh with inconsistent normals.
- After boolean operations that may produce mixed-orientation triangles.
- When **Evaluate & Repair** reports "Orientations" problems.

!!! note "Ambiguous for non-solid meshes"
    Harmonize Normals requires a manifold, connected mesh to work
    reliably. On open meshes (with holes) or meshes with multiple
    components, the result may be inconsistent. Fix holes first.

### Python API

```python
import Mesh

mesh = App.ActiveDocument.getObject("Mesh")
mesh.Mesh.harmonizeNormals()
mesh.touch()
App.ActiveDocument.recompute()
```

---

## Flip Normals

**Menu:** Mesh → Meshes → Flip Normals  
**Command:** `Mesh_FlipNormals`

Reverses the orientation of **all** triangles in the mesh. Use when
the mesh is uniformly inside-out — all normals pointing inward instead
of outward.

If only some normals are wrong (inconsistent), use **Harmonize Normals**
instead. If the mesh is consistently inside-out (Harmonize already ran
but in the wrong direction), use **Flip Normals** after.

### Python API

```python
import Mesh

mesh = App.ActiveDocument.getObject("Mesh")
mesh.Mesh.flipNormals()
mesh.touch()
App.ActiveDocument.recompute()
```

---

## Fill Up Holes

**Menu:** Mesh → Meshes → Fill-up Holes  
**Command:** `Mesh_FillupHoles`

Automatically detects all open boundary loops (holes in the mesh surface)
and fills them with new triangles.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Max hole size | Only fill holes with fewer than N boundary edges |

Set **Max hole size** to a large value (e.g. 1000) to fill all holes
regardless of size. Set it lower to avoid accidentally filling large
openings that are intentional (e.g. the open bottom of a vase).

### Filling algorithm

FreeCAD uses a simple fan triangulation to fill each hole:
- A centre point is computed (average of boundary vertices).
- Triangles connect each boundary edge to the centre.

This is fast but may produce slightly concave fills for curved boundaries.
For better fills, use **Fill Hole Interactively** with advanced options.

### Python API

```python
import Mesh

mesh_obj = App.ActiveDocument.getObject("Mesh")
mesh = mesh_obj.Mesh.copy()
mesh.fillupHoles(10)   # fill holes with up to 10 boundary edges
mesh_obj.Mesh = mesh
App.ActiveDocument.recompute()
```

---

## Fill Hole Interactively

**Menu:** Mesh → Meshes → Close Hole  
**Command:** `Mesh_FillInteractiveHole`

Activates a pick mode where you click on a hole boundary edge to fill
that specific hole. Gives more control than **Fill Up Holes** when the
mesh has a mix of intentional openings and defect holes.

### Step-by-step

1. Activate **Mesh → Meshes → Close Hole**.
2. Click on an edge that borders a hole (the red open boundary lines).
3. FreeCAD fills the hole and returns to pick mode.
4. Repeat for additional holes.
5. Press **Escape** or right-click to exit.

---

## Add Facet

**Menu:** Mesh → Meshes → Add Triangle  
**Command:** `Mesh_AddFacet`

Allows you to manually add a single triangle to the mesh by clicking
three vertices in the 3-D view. Used for fine-grained manual repair
of defects that automated tools cannot fix.

### Step-by-step

1. Activate **Mesh → Meshes → Add Triangle**.
2. Click the first vertex of the new triangle.
3. Click the second vertex.
4. Click the third vertex.
5. The triangle is added.
6. Repeat or press **Escape** to exit.

The new triangle is appended to the mesh without checking for manifold
integrity — verify with **Evaluate & Repair** after manual editing.

---

## Remove Components

**Menu:** Mesh → Meshes → Remove Components  
**Command:** `Mesh_RemoveComponents`

Opens the Remove Components dialog which shows a list of all connected
components (separate mesh islands) in the selected mesh, sorted by
triangle count. Components below a user-set threshold can be deleted
automatically.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Threshold | Remove all components with fewer triangles than this value |

### When to use

After importing a scan, there are often many small noise fragments —
stray triangles, disconnected bits from occlusions, or partial surfaces
of nearby objects. Set the threshold to the minimum reasonable component
size (e.g. 100 triangles) to delete all the noise at once.

---

## Remove Component by Hand

**Menu:** Mesh → Meshes → Remove Component by hand  
**Command:** `Mesh_RemoveCompByHand`

Activates a pick mode where you click on mesh components to delete them
individually. Better than **Remove Components** when you need to keep
some small components and delete others of similar size.

### Step-by-step

1. Activate **Remove Component by hand**.
2. Click on a mesh region to select its component.
3. The selected component is highlighted.
4. Press **Delete** to remove the component.
5. Repeat for other components.
6. Press **Escape** to exit.

---

## Smoothing

**Menu:** Mesh → Meshes → Smooth  
**Command:** `Mesh_Smoothing`

Applies a smoothing filter to the mesh to reduce surface noise and
rough triangulation artifacts. Two algorithms are available:

### Smoothing algorithms

| Algorithm | Description | Effect |
|-----------|-------------|--------|
| Laplacian | Moves each vertex to the centroid of its neighbours | Aggressive — shrinks the mesh slightly over many iterations |
| Taubin | Two-step Laplacian with anti-shrinkage correction | Preserves volume better than Laplacian |

### Parameters

| Parameter | Description |
|-----------|-------------|
| Algorithm | Laplacian or Taubin |
| Iterations | Number of smoothing passes (1–20 typical) |
| Lambda | Smoothing factor (0–1); larger = more smoothing per pass |
| Mu | Anti-shrinkage factor (Taubin only); negative value |

**Recommended settings for scan data repair:**
- Algorithm: Taubin
- Iterations: 5–10
- Lambda: 0.5, Mu: −0.53

More iterations → smoother surface but more detail lost. Sharp edges
and fine features are blurred. Do not over-smooth.

---

## Decimating

**Menu:** Mesh → Meshes → Decimate  
**Command:** `Mesh_Decimating`

Reduces the number of triangles in the mesh while trying to preserve
the overall shape. Decimation is useful when:

- An STL file has more triangles than the 3-D printer needs.
- A scan mesh is too large to handle efficiently.
- You need to reduce file size for web or game use.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Reduction (%) | Target reduction in triangle count |
| Tolerance | Maximum allowable deviation from original surface (mm) |

**Example:** Setting Reduction = 50% and Tolerance = 0.1 mm reduces
the triangle count by 50% while ensuring no point on the simplified mesh
is more than 0.1 mm from the original surface.

### Decimate with tolerance vs percentage

- **Reduction %** targets a specific count reduction regardless of surface error.
- **Tolerance** maintains accuracy and stops decimating when the error limit is reached.

For 3-D printing, tolerance-based decimation is better — it ensures
the print quality is not compromised.

---

## Scale

**Menu:** Mesh → Meshes → Scale  
**Command:** `Mesh_Scale`

Scales the selected mesh by a numeric factor.

### When to use

- Correcting unit errors: an STL imported in inches is 25.4× too small —
  scale by 25.4.
- Scaling up a design: scale by 2 for 2× the size.
- Uniform scale: one factor applied to all three axes.

!!! note "Scale is not parametric"
    Scale modifies the mesh in place. There is no history entry to undo
    the scale later. If you need to return to the original scale, undo
    with Ctrl+Z or re-import.

### Python API

```python
import Mesh

mesh_obj = App.ActiveDocument.getObject("Mesh")
msh = mesh_obj.Mesh.copy()
msh.scale(25.4)   # inches to mm
mesh_obj.Mesh = msh
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Over-smoothing destroys sharp features"
    Smoothing is irreversible (on the current mesh). Sharp edges, lettering,
    and fine detail are blurred and lost. Always keep a backup copy or
    work on a copy of the mesh, not the original import.

!!! warning "Decimation below 10% introduces visible facets"
    Very aggressive decimation makes the triangles visible as flat panels
    on what should be a smooth surface. For printing at 0.2 mm layer
    height, the triangles need to be smaller than 0.2 mm — don't decimate
    below the resolution needed for the print.

!!! warning "Fill Up Holes uses flat triangulation — may look wrong on curved surfaces"
    The automatic hole fill uses a fan triangulation that produces a flat or
    slightly concave patch regardless of the surrounding curvature. For
    aesthetic models, inspect and manually correct large filled areas.

!!! warning "Remove Components is irreversible without undo"
    Removing components modifies the mesh in place. If you accidentally
    remove a component you need, undo immediately (Ctrl+Z). After saving
    or closing, there is no recovery.

---

## See also

- [Analyse](analyse.md) — find defects before repairing
- [Import and Export](import-export.md) — import the mesh to modify
- [Segmentation](segmentation.md) — split the mesh before component removal
