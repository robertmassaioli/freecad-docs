# Points Tools

---

## Import Points

**Menu:** Points → Import Points…  
**Command:** `Points_Import`

Imports a point cloud file and creates a `Points::Feature` object in the
active document.

**Supported formats:** `.asc`, `.pcd`, `.ply`, `.e57`

### Step-by-step

1. Go to **Points → Import Points…**.
2. A file dialog opens. Select a point cloud file.
3. Click **Open**.
4. FreeCAD creates a `Points::Feature` in the document.

### Out-of-origin warning

If the bounding box of the imported cloud does not contain the origin,
FreeCAD offers to **translate the cloud to the origin**. Accept this for
most workflows — Coin3D (the 3-D renderer) uses single-precision floats
internally, so a cloud far from the origin (e.g. at a national grid
coordinate) can show rendering artefacts. Moving it to the origin avoids
floating-point precision issues.

### Python API

```python
import Points

# Import a file
Points.insert("/path/to/scan.ply", "Document")
# or for .e57
Points.insert("/path/to/scan.e57", "Document")
```

---

## Export Points

**Menu:** Points → Export Points…  
**Command:** `Points_Export`

Exports the selected `Points::Feature` object(s) to a file.

**Supported formats:** `.asc`, `.pcd`, `.ply`

!!! note
    The E57 format is supported for **import only**. To save in E57,
    use a dedicated point cloud application.

### Step-by-step

1. Select one or more `Points::Feature` objects in the document tree.
2. Go to **Points → Export Points…**.
3. For each selected object, a save dialog appears. Choose a file name
   and format.
4. Click **Save**.

### Python API

```python
import Points

cloud = App.ActiveDocument.getObject("PointsFeature")
Points.export([cloud], "/path/to/output.ply")
```

---

## Convert to Points

**Menu:** Points → Convert to Points  
**Command:** `Points_Convert`

Samples points from any geometric object (mesh or B-rep solid) and creates a
`Points::Feature`. This is the bridge from CAD geometry to point cloud data.

### Parameters

A single dialog asks for **maximum distance** (tolerance) — the maximum
spacing between sampled points on the surface. Smaller values produce
denser, more accurate clouds; larger values produce sparser clouds.

| Tolerance | Effect |
|-----------|--------|
| Small (e.g. 0.01 mm) | Dense cloud, close to the true surface, longer computation |
| Large (e.g. 1.0 mm) | Sparse cloud, faster, may miss fine detail |

Default: 0.1 mm.

### Step-by-step

1. Select one or more geometric objects (mesh features or Part solids).
2. Go to **Points → Convert to Points**.
3. Enter the maximum distance in the dialog.
4. Click **OK**. FreeCAD creates a `Points::Feature` for each selected object.

### Python API

```python
# Use the pointscommands helper module
import pointscommands.commands as pc
import App

solid = App.ActiveDocument.getObject("Body")
pc.make_points_from_geometry([solid], 0.1)   # tolerance = 0.1 mm
App.ActiveDocument.recompute()
```

---

## Structured Point Cloud

**Menu:** Points → Structured Point Cloud  
**Command:** `Points_Structure`

Converts a `Points::Feature` (unstructured) into a `Points::Structured`
object — a Width × Height grid where each cell corresponds to one sensor
pixel. Points that have no valid scan return are stored as `NaN` coordinates.

This is useful for:

- Working with data from structured-light scanners or depth cameras that
  produce organised image-like outputs.
- Processing algorithms that expect a regular grid topology.

### How it works

The tool analyses the X and Y coordinates of all points to count unique
X values (→ Width) and unique Y values (→ Height). It then fills a
Width × Height grid, placing each point at its grid cell. Cells with no
corresponding input point get a `NaN` placeholder.

!!! warning "Only works for truly grid-structured input"
    The algorithm assumes that the input points lie on a regular grid —
    i.e. their X and Y values are multiples of a common step size. For
    an arbitrary unordered scan (e.g. from a handheld LiDAR), the result
    is meaningless. Use this tool only on data from grid-producing sensors.

### Step-by-step

1. Select a single `Points::Feature` object.
2. Go to **Points → Structured Point Cloud**.
3. FreeCAD creates a `Points::Structured` object named
   `<original name> (Structured)`.

### Resulting object properties

| Property | Type | Description |
|----------|------|-------------|
| `Points` | PropertyPointKernel | The Width × Height ordered point array |
| `Width` | Integer | Number of columns |
| `Height` | Integer | Number of rows |

Invalid cells have coordinates `(NaN, NaN, NaN)`.

---

## Cut Point Cloud

**Menu:** Points → Cut Point Cloud  
**Command:** `Points_PolyCut`

Enters a lasso-selection mode that lets you draw a closed polygon in the 3-D
view. Points inside the polygon are removed from the cloud.

### Step-by-step

1. Select a `Points::Feature` object.
2. Go to **Points → Cut Point Cloud**.
3. The cursor changes to a lasso.
4. Click in the 3-D view to place polygon vertices. Each click adds a vertex.
5. Double-click (or close the polygon) to confirm the selection.
6. Points inside the lasso are deleted from the cloud.
7. Press **Escape** to cancel without deleting.

!!! note "The cut is non-destructive to the file"
    The cut modifies the in-memory point kernel. If you want to keep the
    original, export the cloud before cutting.

---

## Merge Point Clouds

**Menu:** Points → Merge Point Clouds  
**Command:** `Points_Merge`

Combines two or more selected `Points::Feature` objects into a single new
`Points::Feature` named "Merged Points".

The merged cloud inherits optional attributes (colour, normals, intensity)
from the input clouds if they are consistent:

| Attribute | Behaviour |
|-----------|-----------|
| `Color` | Copied if all inputs have colour → DisplayMode set to "Color" |
| `Normal` | Copied if all inputs have normals → DisplayMode set to "Shaded" |
| `Intensity` | Copied if all inputs have intensity → DisplayMode set to "Intensity" |

If inputs have inconsistent attributes (e.g. one with colour, one without),
the merged cloud has no attribute data and renders in default mode.

### Step-by-step

1. Select **two or more** `Points::Feature` objects (Ctrl+click in the tree).
2. Go to **Points → Merge Point Clouds**.
3. FreeCAD creates a new `Points::Feature` containing all points from all
   selected clouds.

### Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
cloud1 = doc.getObject("Cloud1")
cloud2 = doc.getObject("Cloud2")

merged = doc.addObject("Points::Feature", "Merged")
kernel = merged.Points.startEditing()

for src in [cloud1, cloud2]:
    k = src.Points.getValue()
    n = kernel.size()
    kernel.resize(n + k.size())
    for i in range(k.size()):
        kernel.setPoint(n + i, k.getPoint(i))

merged.Points.finishEditing()
doc.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Import produces a cloud far from the origin — rendering artefacts"
    **Cause:** Survey or geo-referenced data uses absolute coordinates (e.g.
    national grid) so the cloud is kilometres from the origin. Coin3D renders
    in single-precision floating point; large offsets cause jitter.  
    **Fix:** Accept the "Translate to origin?" prompt at import, or move the
    cloud manually.

!!! warning "Structured Point Cloud produces wrong Width/Height"
    **Cause:** The algorithm counts distinct X and Y values. If the input has
    numerical noise (X values that should be identical differ by 1e-10), it
    overcounts the grid dimensions.  
    **Fix:** Clean the cloud before structuring — round X and Y to the expected
    grid step, or use the structured import option in your scanner software.

!!! warning "Cut Point Cloud removes the wrong points"
    **Cause:** The lasso is projected from the current camera viewpoint. Points
    that appear inside the polygon from one angle may be on the far side of the
    model.  
    **Fix:** Rotate to a view where the target region is clearly separated in
    screen space before drawing the lasso.

!!! warning "Merge drops colour data"
    **Cause:** If any input cloud lacks a `Color` property, the merged cloud
    is created without colour.  
    **Fix:** Ensure all input clouds have consistent attributes before merging,
    or add colour programmatically after merging.
