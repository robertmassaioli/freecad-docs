# Import Points

> **In one sentence:** Load a point cloud file from disk and create a
> `Points::Feature` object in the active document.

---

## Overview

**Workbench:** Points  
**Menu:** Points → Import Points…  
**Shortcut:** none

Reads a point cloud file and adds it to the current document as a
`Points::Feature` object, ready for visualisation, processing, or export.
Supported formats are `.asc`, `.pcd`, `.ply`, and `.e57`.

---

## Intuition

A point cloud is just a large list of 3-D coordinates — the output of a
LiDAR scanner, a structured-light system, or photogrammetry software. FreeCAD
itself cannot *create* point clouds, but Import Points lets you bring in
clouds produced by external hardware or software and work with them alongside
CAD geometry.

Think of it as a straightforward file-open: you pick a supported cloud file
and FreeCAD reads the coordinates into a lightweight `Points::Feature` object
that you can visualise and pass to the other Points tools.

---

## When to use it

- You have a scan file (`.ply`, `.pcd`, `.asc`, `.e57`) from a LiDAR scanner
  or photogrammetry pipeline that you want to bring into FreeCAD.
- You need to compare a manufactured part against its scan cloud.
- You want to use the cloud as a reference for curve-on-mesh or reverse-engineering workflows.

---

## When NOT to use it

- **Don't use this to open mesh files (STL, OBJ, etc.)** — those are triangle
  meshes, not point clouds. Use the Mesh workbench → Import Mesh instead.
- **Don't use this for CAD formats (STEP, IGES, FCStd)** — use File → Open or
  File → Import.

---

## Step-by-step

1. Switch to the **Points workbench**.
2. Choose **Points → Import Points…**.
3. A file dialog opens. Navigate to your point cloud file.
4. Select the file and click **Open**.
5. FreeCAD creates a `Points::Feature` object in the document tree.
6. If the cloud's bounding box does not contain the origin, FreeCAD asks
   whether to **translate the cloud to the origin**. Accept for most
   workflows (see note below).

!!! tip
    Survey and geo-referenced clouds are often millions of metres from the
    origin (absolute coordinates). Always translate to origin on import —
    FreeCAD's 3-D renderer uses single-precision floats internally, which
    causes visible jitter for large coordinate values.

---

## Parameters

This tool has no task panel — it opens a file dialog directly. The only
parameter is the file path chosen in the dialog.

| Input | Description |
|-------|-------------|
| File path | Path to the point cloud file to import |
| Translate to origin? | Dialog prompt shown when the cloud bounding box does not contain the origin. Selecting Yes translates the entire cloud so its bounding-box centre is at the document origin. |

### Supported file formats

| Extension | Format | Notes |
|-----------|--------|-------|
| `.asc` | ASCII XYZ | One point per line, whitespace-separated X Y Z |
| `.pcd` | Point Cloud Data | PCL library format; supports colour and normals |
| `.ply` | Polygon File Format | Binary or ASCII; supports colour and normals |
| `.e57` | ASTM E57 | Standard scanner exchange format |

---

## Common mistakes and pitfalls

!!! warning "Rendering jitter on distant clouds"
    **Cause:** Geo-referenced data has absolute coordinates far from the
    origin. FreeCAD's renderer uses 32-bit floats; large values lose
    precision.  
    **Fix:** Accept the "Translate to origin?" prompt, or move the cloud
    manually with the Placement property after import.

!!! warning "File not found / format not supported"
    **Cause:** File extension is recognised but the content does not match
    (e.g. a binary PCD file that FreeCAD's reader cannot parse).  
    **Fix:** Convert the file using CloudCompare or open3d before importing.

!!! warning "E57 files import but appear empty"
    **Cause:** Some E57 files store intensity or RGB channels but no XYZ
    data that FreeCAD can read.  
    **Fix:** Open the E57 file in CloudCompare, confirm it has XYZ data,
    and export as PCD or PLY before importing into FreeCAD.

---

## Python API

```python
import Points

# Import a PLY file into the active document
Points.insert("/path/to/scan.ply", "Document")

# Import an E57 file
Points.insert("/path/to/scan.e57", "Document")

# The resulting object is a Points::Feature in App.ActiveDocument
import FreeCAD as App
cloud = App.ActiveDocument.ActiveObject
print(cloud.Points.Count)   # number of points imported
```

---

## See also

- [Export Points](export-points.md) — save a cloud back to disk
- [Merge Point Clouds](merge-point-clouds.md) — combine multiple imported clouds
- [Structured Point Cloud](structured-point-cloud.md) — reorganise a grid-scan cloud
- [Points Workbench](../index.md) — overview of the Points workbench
