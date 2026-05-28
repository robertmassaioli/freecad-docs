# Export Points

> **In one sentence:** Save a `Points::Feature` object from the document to a
> point cloud file on disk.

---

## Overview

**Workbench:** Points  
**Menu:** Points → Export Points…  
**Shortcut:** none

Writes one or more selected `Points::Feature` objects to files. For each
selected cloud, a save dialog appears in turn asking for the file name and
format. Supported output formats are `.asc`, `.pcd`, and `.ply`.

---

## Intuition

Export Points is the reverse of Import Points: it takes a cloud that lives
inside FreeCAD (possibly modified, cut, or merged) and writes the coordinates
back to a file that other software can read. Think of it as a Save As for
point cloud data.

---

## When to use it

- You have modified a cloud inside FreeCAD (cut, merged, converted from
  geometry) and want to save the result for use in another application.
- You want to pass a cloud from FreeCAD to CloudCompare, Meshroom, or a
  custom processing pipeline.
- You need a PLY file of a cloud for 3-D printing or web visualisation.

---

## When NOT to use it

- **Don't use this to save the FreeCAD document** — use File → Save (`.FCStd`).
- **Don't expect E57 output** — the E57 format is supported for import only.
  For E57 export, use CloudCompare.
- **Don't use this for mesh export** — if you need an STL or OBJ, use the
  Mesh workbench → Export Mesh.

---

## Step-by-step

1. Select one or more `Points::Feature` objects in the document tree.
2. Choose **Points → Export Points…**.
3. A save dialog appears for the first selected cloud. Choose a file name
   and format (`.asc`, `.pcd`, or `.ply`).
4. Click **Save**.
5. If you selected multiple clouds, the dialog repeats for each one.

---

## Parameters

| Input | Description |
|-------|-------------|
| Selection | One or more `Points::Feature` objects |
| File path | The destination file path chosen in the save dialog |
| Format | Determined by the file extension you type in the dialog |

### Supported output formats

| Extension | Format | Notes |
|-----------|--------|-------|
| `.asc` | ASCII XYZ | Plain-text, one point per line; universally readable |
| `.pcd` | Point Cloud Data | PCL format; supports colour and normals if present |
| `.ply` | Polygon File Format | Binary or ASCII; supports colour and normals if present |

!!! note "E57 not supported for export"
    The E57 format can be imported but not exported by FreeCAD. For E57
    output, load the PLY file in CloudCompare and re-save as E57.

---

## Common mistakes and pitfalls

!!! warning "Multiple clouds produce multiple dialogs"
    **Cause:** Each selected cloud opens its own save dialog.  
    **Fix:** This is expected behaviour. Name each file before clicking Save.
    If you want a single file, merge the clouds first with
    [Merge Point Clouds](merge-point-clouds.md).

!!! warning "Colour data not preserved in ASC format"
    **Cause:** The ASC format stores only XYZ coordinates.  
    **Fix:** Export as PLY or PCD if you need to preserve RGB, normals, or
    intensity attributes.

---

## Python API

```python
import Points
import FreeCAD as App

doc = App.ActiveDocument
cloud = doc.getObject("PointsFeature")   # replace with actual object name

# Export to PLY
Points.export([cloud], "/path/to/output.ply")

# Export multiple clouds into separate files
cloud2 = doc.getObject("PointsFeature001")
Points.export([cloud],  "/path/to/cloud1.ply")
Points.export([cloud2], "/path/to/cloud2.ply")
```

---

## See also

- [Import Points](import-points.md) — load a cloud from disk
- [Merge Point Clouds](merge-point-clouds.md) — combine clouds before exporting
- [Points Workbench](../index.md) — overview of the Points workbench
