# OpenSCAD Tools

---

## Replace Object {#replace-object}

**Menu:** OpenSCAD → Replace Object  
**Command:** `OpenSCAD_ReplaceObject`

Replaces one object with another in the document dependency graph. When you
redesign a base shape that is used by many dependent features, Replace Object
wires the new shape into all the places the old one was referenced, without
manually re-linking each reference.

### Selection

Select **2 or 3** `Part::Feature` objects:

| Selection pattern | Behaviour |
|-------------------|-----------|
| 3 objects: old, new, parent | Replaces `old` with `new` inside `parent` |
| 2 objects where one has 0 parents and the other has 1 | Replaces the independent object with the one that has a parent |

---

## Remove Objects and Children {#remove-objects-and-children}

**Menu:** OpenSCAD → Remove Objects and Children  
**Command:** `OpenSCAD_RemoveSubtree`

Deletes the selected objects **and** all objects in their subtree that are not
referenced by anything else. Unlike pressing Delete (which only removes the
top-level object and leaves orphaned children), this tool cleans up the entire
unused subtree in one step.

Useful when clearing the result of a failed boolean or an experimental branch
of the document tree.

---

## Refine Shape Feature {#refine-shape-feature}

**Menu:** OpenSCAD → Refine Shape Feature  
**Command:** `OpenSCAD_RefineShapeFeature`

Creates a `Part::FeaturePython` (named `refine_<original>`) that wraps the
selected shape and removes **seam edges** — redundant edges on smooth faces
left behind by boolean operations. The original object is hidden.

This is equivalent to enabling **Refine** in Part's Boolean operations, but
applies it as a separate, explicit feature that can be deleted or bypassed.

**When to use:** After a Boolean Cut or Fuse that leaves unwanted lines across
a smooth face. Also useful before exporting to STEP, where clean topology
is expected.

---

## Mirror Mesh Feature {#mirror-mesh-feature}

**Menu:** OpenSCAD → Mirror Mesh Feature  
**Command:** `OpenSCAD_MirrorMeshFeature`

Mirrors a selected mesh about a user-specified axis. A dialog prompts for the
mirror axis as a vector `[x;y;z]`. Preset options include the three principal
axes and diagonal combinations.

The original mesh is hidden and a new `Mesh::Feature` named `mirror_<original>`
is created with the mirrored geometry.

---

## Scale Mesh Feature {#scale-mesh-feature}

**Menu:** OpenSCAD → Scale Mesh Feature  
**Command:** `OpenSCAD_ScaleMeshFeature`

Scales a selected mesh by independent factors along X, Y, and Z. A dialog
prompts for the scale vector `[sx;sy;sz]`. The default `[1;1;1]` is a
no-op; enter e.g. `[2;1;0.5]` to double the X size and halve the Z size.

The original is hidden and a new `Mesh::Feature` named `scale_<original>`
is created.

!!! note "Scale vs Resize"
    **Scale** multiplies each axis by a factor (dimensionless). **Resize**
    sets the mesh to a specific absolute size in mm (see below).

---

## Resize Mesh Feature {#resize-mesh-feature}

**Menu:** OpenSCAD → Resize Mesh Feature  
**Command:** `OpenSCAD_ResizeMeshFeature`

Resizes a mesh to absolute target dimensions. Enter the target size as
`[lx;ly;lz]` in mm. The mesh is uniformly scaled so that its bounding box
matches those dimensions.

---

## Increase Tolerance Feature {#increase-tolerance-feature}

**Menu:** OpenSCAD → Increase Tolerance Feature  
**Command:** `OpenSCAD_IncreaseToleranceFeature`

Creates a `Part::FeaturePython` wrapper that increases the OCCT geometric
tolerance of the underlying shape. This is a repair tool for shapes that fail
boolean operations because near-coincident vertices or edges fall just outside
the default 1e-7 mm tolerance.

After applying, retry the boolean operation. If the operation now succeeds,
the tolerance was the root cause.

!!! warning
    Increasing tolerance makes the shape geometry less precise. Use only as
    a workaround when other repair methods fail.

---

## Convert Edges to Faces {#convert-edges-to-faces}

**Menu:** OpenSCAD → Convert Edges to Faces  
**Command:** `OpenSCAD_Edgestofaces`

Takes the edges of selected `Part::Feature` objects and builds faces from
closed edge loops. This is useful for recovering 2-D geometry (a set of
edges that enclose a region) as proper face objects.

The tool uses an overlap-detection algorithm to handle cases where multiple
edge loops share boundary segments.

---

## Expand Placements {#expand-placements}

**Menu:** OpenSCAD → Expand Placements  
**Command:** `OpenSCAD_ExpandPlacements`

Propagates placement values from parent objects down into their children,
effectively "baking in" the placement hierarchy. After running, each child
object has an absolute placement and the parent is at the origin.

!!! warning
    This operation is not fully reversible and can break extrusions and other
    features that depend on relative placement. Use with caution.

---

## Explode Group {#explode-group}

**Menu:** OpenSCAD → Explode Group  
**Command:** `OpenSCAD_ExplodeGroup`

Breaks apart a `Part::Fuse`, `Part::MultiFuse`, or `Part::Compound` into its
individual component objects. The fusion/compound object is deleted and each
sub-object is shown individually.

If a component has the default shape colour, Explode Group assigns it a random
colour so the components are visually distinguishable.

**Constraint:** Only works on top-level objects (objects with no parent in the
document tree).

---

## Add OpenSCAD Element {#add-openscad-element}

**Menu:** OpenSCAD → Add OpenSCAD Element  
**Command:** `OpenSCAD_AddOpenSCADElement`  
**Requires:** OpenSCAD binary

Opens a task panel with a code editor where you can type OpenSCAD code
directly. Click **Add** to call the OpenSCAD binary, convert the code to
geometry, and import the result into FreeCAD.

### Task panel controls

| Control | Description |
|---------|-------------|
| **Code editor** | Type OpenSCAD code here. Default: `cube();` |
| **Add** | Runs OpenSCAD on the code and imports the result |
| **Refresh** | Clears the document and re-runs Add |
| **Open…** | Load a `.scad` or `.csg` file into the editor |
| **Save…** | Save the current code to a `.scad` or `.csg` file |
| **Clear Code** | Empty the code editor |
| **as mesh** checkbox | Import result as `Mesh::Feature` (STL) instead of CSG. Use for code that generates non-manifold geometry. |

### Typical uses

- Quickly prototype OpenSCAD shapes and import them into a FreeCAD assembly.
- Test that an existing `.scad` file imports correctly.
- Use OpenSCAD primitives (e.g. `minkowski`, `hull`) without leaving FreeCAD.

---

## Mesh Boolean {#mesh-boolean}

**Menu:** OpenSCAD → Mesh Boolean  
**Command:** `OpenSCAD_MeshBoolean`  
**Requires:** OpenSCAD binary

Performs a boolean operation on the selected objects using OpenSCAD's CGAL
boolean engine. The input objects are converted to STL meshes, passed to
OpenSCAD, and the result is imported back as a `Mesh::Feature`.

### Operations

| Operation | Description |
|-----------|-------------|
| **Union** | Combined volume of all selected objects |
| **Intersection** | Volume common to all selected objects |
| **Difference** | First object minus all subsequent objects |
| **Hull** | Convex hull (same as the Hull tool) |
| **Minkowski sum** | Minkowski sum (same as the Minkowski Sum tool) |

### Why use this instead of Part Boolean?

OpenSCAD's CGAL mesh booleans are **more robust** than OCCT's B-rep booleans
for certain inputs — especially meshes with self-intersections or near-zero
thickness faces. If a Part Boolean fails with "invalid shape", try Mesh Boolean
as a fallback.

The trade-off is that the result is a mesh, not a B-rep solid — it cannot be
used directly in Part Design without converting back via **Part → Create Shape
from Mesh**.

---

## Hull {#hull}

**Menu:** OpenSCAD → Hull  
**Command:** `OpenSCAD_Hull`  
**Requires:** OpenSCAD binary

Computes the **convex hull** of all selected objects. The convex hull is the
smallest convex solid that contains all the selected shapes.

The result is imported as a `Part` object (via CSG import). All selected
objects are hidden.

**Example uses:**
- Wrapping multiple parts in a single bounding solid for interference checking.
- Creating a convex approximation of a complex shape for collision detection.

---

## Minkowski Sum {#minkowski-sum}

**Menu:** OpenSCAD → Minkowski Sum  
**Command:** `OpenSCAD_Minkowski`  
**Requires:** OpenSCAD binary

Computes the **Minkowski sum** of the selected objects. The Minkowski sum of
shapes A and B is the set of all points A + B (vector sum). In practice:

- Minkowski(solid, sphere of radius r) = solid expanded ("offset") by radius r
  with rounded corners.
- Minkowski(shape, small cube) = shape with flat-corner expansion.

This is a powerful morphological operation for creating rounded offsets or
for generating "swept" volumes.

The result is imported as a `Part` object and all inputs are hidden.

---

## Common mistakes and pitfalls

!!! warning "Mesh Boolean fails with 'OpenSCAD executable not found'"
    **Cause:** The OpenSCAD binary is not configured.  
    **Fix:** Install OpenSCAD from openscad.org, then set the path in
    **Edit → Preferences → OpenSCAD → OpenSCAD executable**.

!!! warning "Hull / Minkowski produce an unexpected result"
    **Cause:** The operation was run on objects with non-trivial placements
    that were not accounted for.  
    **Fix:** Ensure all input objects are at their final placements before
    applying Hull or Minkowski. Use **Expand Placements** if needed.

!!! warning "Add OpenSCAD Element fails to load the temp file (Snap/AppImage)"
    **Cause:** Snap and AppImage versions of OpenSCAD or FreeCAD run in
    sandboxes that may not share the `/tmp` directory.  
    **Fix:** In **Edit → Preferences → OpenSCAD**, change **Transfer mechanism**
    from "Temp directory" to an alternative that both applications can access.

!!! warning "Refine Shape Feature leaves visible edges"
    **Cause:** Some edges that appear seam-like are actually topological edges
    needed for the shape (e.g. where two flat faces meet at a sharp angle).
    Refine removes only truly redundant edges.  
    **Fix:** Use Part → Check Geometry to confirm the shape is valid. If edges
    remain after refine, they are real edges — accept them or use a fillet.
