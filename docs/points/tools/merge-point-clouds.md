# Merge Point Clouds

> **In one sentence:** Combine two or more `Points::Feature` objects into a
> single new cloud containing all their points.

---

## Overview

**Workbench:** Points  
**Menu:** Points → Merge Point Clouds  
**Shortcut:** none

Creates a new `Points::Feature` named "Merged Points" containing every point
from all selected input clouds. Colour, normal, and intensity attributes are
preserved if all inputs carry the same attribute type; otherwise the merged
cloud has no attributes.

---

## Intuition

Think of it as appending several text files together into one file. Each cloud
contributes its full list of points; the result is one big list. There is no
registration step — Merge Point Clouds assumes the input clouds are already in
a common coordinate frame (i.e. they were imported or aligned together).

---

## When to use it

- You have scanned an object from multiple stations and aligned the scans
  externally (in CloudCompare, for example); now you want one unified cloud
  in FreeCAD.
- You split a cloud with [Cut Point Cloud](cut-point-cloud.md) and want to
  reassemble subsets.
- You have converted multiple CAD parts to clouds and want a single reference
  cloud.
- You want to export several clouds as one file with [Export Points](export-points.md).

---

## When NOT to use it

- **Don't use this to register (align) unaligned scans** — FreeCAD has no
  ICP or feature-based registration tool. Register the clouds in CloudCompare
  or another tool first, then merge in FreeCAD.
- **Don't merge clouds with inconsistent attributes if you need colour** —
  if one cloud has RGB and another does not, the merged cloud loses all
  colour information.

---

## Step-by-step

1. Select **two or more** `Points::Feature` objects in the model tree (hold
   Ctrl to multi-select).
2. Choose **Points → Merge Point Clouds**.
3. FreeCAD creates a new `Points::Feature` named "Merged Points" in the
   document tree.
4. The input clouds remain in the document — they are not deleted.

---

## Parameters

This tool has no parameter dialog. The only input is the set of selected
`Points::Feature` objects.

### Attribute merging rules

| Attribute | Behaviour |
|-----------|-----------|
| `Color` (RGB) | Carried to merged cloud if **all** inputs have colour; dropped otherwise |
| `Normal` | Carried to merged cloud if **all** inputs have normals; dropped otherwise |
| `Intensity` | Carried to merged cloud if **all** inputs have intensity; dropped otherwise |

---

## Common mistakes and pitfalls

!!! warning "Merged cloud loses colour data"
    **Cause:** At least one of the input clouds has no `Color` property. The
    merge requires all inputs to have consistent attributes.  
    **Fix:** Ensure every cloud that has colour data is paired only with other
    colour clouds. If you must merge a coloured and an uncoloured cloud,
    add dummy RGB values to the uncoloured cloud in Python before merging.

!!! warning "Merged cloud appears at wrong location"
    **Cause:** The clouds have different placements (Placement property differs).
    Merge Point Clouds does not account for object placement transforms — it
    takes the raw point coordinates as stored.  
    **Fix:** Apply the placement before merging. Select each cloud, open its
    Placement property, and reset it to zero, or use a macro to apply the
    transformation to the point coordinates.

!!! warning "Selecting non-Points objects causes an error"
    **Cause:** The tool only accepts `Points::Feature` objects. Selecting a
    mesh, solid, or other object type causes it to fail silently or report an
    error.  
    **Fix:** Ensure only `Points::Feature` objects are selected.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
cloud1 = doc.getObject("Cloud1")
cloud2 = doc.getObject("Cloud2")

# Create the merged cloud manually
merged = doc.addObject("Points::Feature", "Merged")
kernel1 = cloud1.Points.getValue()
kernel2 = cloud2.Points.getValue()

import Points as Pts
new_kernel = Pts.Points()
for i in range(kernel1.size()):
    new_kernel.addPoint(kernel1.getPoint(i))
for i in range(kernel2.size()):
    new_kernel.addPoint(kernel2.getPoint(i))

merged.Points = new_kernel
doc.recompute()
print("Merged cloud has", merged.Points.Count, "points")
```

---

## See also

- [Import Points](import-points.md) — load the clouds to merge
- [Cut Point Cloud](cut-point-cloud.md) — remove regions before or after merging
- [Export Points](export-points.md) — save the merged cloud
- [Points Workbench](../index.md) — overview of the Points workbench
