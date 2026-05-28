# Cut Point Cloud

> **In one sentence:** Draw a lasso polygon in the 3-D view to delete all
> points inside the selected region from a point cloud.

---

## Overview

**Workbench:** Points  
**Menu:** Points → Cut Point Cloud  
**Shortcut:** none

Enters an interactive lasso-selection mode. You click to place polygon vertices
in the 3-D view; when you close the polygon, all points that project inside the
polygon from the current camera angle are removed from the cloud.

---

## Intuition

Think of it as using scissors on a photograph of your cloud. You draw a closed
outline around the region you want to remove, and everything inside the outline
disappears. The "outline" is always projected from your current viewing angle,
so rotating the view before cutting gives you control over which points are
actually removed in 3-D.

---

## When to use it

- Removing ground points, walls, or background clutter from a scan before
  processing.
- Isolating a region of interest by cutting away everything outside it.
- Quickly cleaning obvious noise or erroneous returns from a cloud.

---

## When NOT to use it

- **Don't use this for precise boundary extraction** — the lasso projects from
  the current camera, so depth ambiguity means points "behind" the target may
  also be cut. Rotate and cut in multiple passes for precise results.
- **Don't expect undo** — the cut modifies the in-memory cloud. Save a copy
  first if you need to recover the original points.

---

## Step-by-step

1. Select a `Points::Feature` object in the document tree.
2. Choose **Points → Cut Point Cloud**.
3. The cursor changes to a lasso cursor.
4. Click in the 3-D view to place the first polygon vertex.
5. Continue clicking to add more vertices around the region to cut.
6. **Double-click** on the last vertex (or click the first vertex again) to
   close the polygon and confirm the cut.
7. Points inside the lasso are deleted from the cloud.
8. Press **Escape** at any time before closing the polygon to cancel.

!!! tip
    Rotate to a viewpoint where the target region is clearly separated in
    screen space before drawing the lasso. For clouds with depth complexity,
    make multiple passes from different angles.

---

## Parameters

This tool has no parameters — it is entirely interactive.

---

## Common mistakes and pitfalls

!!! warning "Wrong points removed due to depth projection"
    **Cause:** The lasso projects from the camera. Points that appear inside
    the polygon on-screen may be at different depths — both a near and a far
    cluster inside the projected outline will be cut.  
    **Fix:** Rotate to a view that separates the target depth plane. Use
    multiple lasso cuts from different angles if the cloud has overlapping
    regions of interest.

!!! warning "Cut is non-reversible"
    **Cause:** The tool deletes points from the in-memory kernel with no undo
    stack.  
    **Fix:** Before cutting, duplicate the cloud (select it and use Edit →
    Duplicate) or export it with [Export Points](export-points.md) to preserve
    the original.

!!! warning "Lasso does not close correctly"
    **Cause:** Clicking outside the 3-D viewport or hitting the wrong key
    may dismiss the lasso tool without cutting.  
    **Fix:** Double-click the last vertex while the cursor is in the 3-D
    viewport to close the lasso.

---

## Python API

The Cut Point Cloud tool is interactive-only and does not have a direct Python
equivalent. For scripted removal of points, manipulate the point kernel directly:

```python
import FreeCAD as App

doc = App.ActiveDocument
cloud = doc.getObject("PointsFeature")
kernel = cloud.Points.getValue()

# Keep only points above Z = 0 (example filter)
kept = [kernel.getPoint(i) for i in range(kernel.size())
        if kernel.getPoint(i).z > 0]

# Write the filtered cloud back
import Points as Pts
new_cloud = doc.addObject("Points::Feature", "FilteredCloud")
new_kernel = Pts.Points()
for pt in kept:
    new_kernel.addPoint(pt)
new_cloud.Points = new_kernel
doc.recompute()
```

---

## See also

- [Import Points](import-points.md) — load a cloud before cutting
- [Merge Point Clouds](merge-point-clouds.md) — recombine regions after cutting
- [Export Points](export-points.md) — save the cut cloud
- [Points Workbench](../index.md) — overview of the Points workbench
