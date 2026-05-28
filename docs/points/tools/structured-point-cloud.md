# Structured Point Cloud

> **In one sentence:** Reorder an unstructured `Points::Feature` into a regular
> Width × Height grid matching the sensor's pixel layout.

---

## Overview

**Workbench:** Points  
**Menu:** Points → Structured Point Cloud  
**Shortcut:** none

Converts a `Points::Feature` (unstructured list of XYZ coordinates) into a
`Points::Structured` object — a 2-D grid where each cell corresponds to one
sensor pixel. Cells with no valid scan return are stored as `NaN` coordinates.

---

## Intuition

Some 3-D sensors — structured-light scanners, time-of-flight cameras, LIDAR
sensors with a known scan pattern — produce point clouds where each point has
a well-defined row and column position in a pixel grid. An unstructured cloud
loses this neighbourhood information; a structured cloud preserves it.

Think of it like a photograph vs a list of pixel colours. The photograph
preserves which pixels are adjacent to each other; the list does not.
Structured Point Cloud takes the list and puts it back into photograph form,
using the X and Y coordinates to figure out which row and column each point
belongs to.

This structure is required by algorithms that exploit neighbourhood topology —
surface normal estimation, hole detection, and per-row/column processing.

---

## When to use it

- Your cloud comes from a structured sensor (depth camera, structured-light
  scanner) and you need neighbourhood topology for processing.
- You want to work with the cloud in a row/column-addressable way from Python.
- A downstream algorithm explicitly requires a `Points::Structured` input.

---

## When NOT to use it

- **Don't use this on a handheld LiDAR or photogrammetry cloud** — those
  clouds have no regular grid topology and the result will be meaningless.
- **Don't use this on a cloud created by [Convert to Points](convert-to-points.md)**
  — the sampled points are not on a regular grid.
- **Don't use this expecting data cleaning** — invalid cells become `NaN`,
  they are not filled or interpolated.

---

## Step-by-step

1. Select a single `Points::Feature` object in the document tree.
2. Choose **Points → Structured Point Cloud**.
3. FreeCAD analyses the X and Y coordinates to determine the grid dimensions.
4. A `Points::Structured` object named `<original name> (Structured)` is
   created in the document tree.

---

## Parameters

This tool has no parameters — the grid dimensions (Width and Height) are
computed automatically from the point coordinates.

### How the grid is inferred

The algorithm counts the number of **unique X values** → Width, and
the number of **unique Y values** → Height. Each input point is placed at the
grid cell `(col, row)` corresponding to its X and Y values. Cells with no
input point receive `NaN` coordinates.

### Resulting object properties

| Property | Type | Description |
|----------|------|-------------|
| `Points` | PropertyPointKernel | The Width × Height ordered point array |
| `Width` | Integer | Number of columns in the grid |
| `Height` | Integer | Number of rows in the grid |

Invalid cells have coordinates `(NaN, NaN, NaN)`.

---

## Common mistakes and pitfalls

!!! warning "Wrong Width or Height due to floating-point noise"
    **Cause:** If the X values of points in the same column differ by tiny
    amounts (e.g. 100.000001 vs 100.000000), they are counted as two distinct
    X values and Width is overcounted.  
    **Fix:** Round X and Y coordinates to the expected grid step before
    running this tool. This can be done in Python or in CloudCompare before
    importing.

!!! warning "Tool produces garbage output on unstructured clouds"
    **Cause:** The grid-inference algorithm assumes the cloud has a regular
    structure. A cloud with random X/Y distributions produces a nonsensical
    huge grid.  
    **Fix:** Only apply Structured Point Cloud to data from grid-producing
    sensors. Check the sensor specifications.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
cloud = doc.getObject("PointsFeature")

# The structured object is created by GUI command
# Access its properties after creation:
structured = doc.getObject("PointsFeature (Structured)")
if structured:
    print("Width:", structured.Width)
    print("Height:", structured.Height)
    pts = structured.Points.getValue()
    # pts is a list of Width*Height (x, y, z) tuples
    # NaN entries mark invalid cells
```

---

## See also

- [Import Points](import-points.md) — import the scan file first
- [Convert to Points](convert-to-points.md) — the unstructured alternative
- [Points Workbench](../index.md) — overview of the Points workbench
