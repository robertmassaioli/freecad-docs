# Visual Inspection

> **In one sentence:** Visual Inspection opens a dialog where you choose an
> *actual* object and one or more *nominal* objects, set the search radius and
> thickness, and FreeCAD computes a signed-distance map displayed as a
> false-colour heat map in the 3-D view.

---

## Overview

**Workbench:** Inspection  
**Menu:** Inspection → Visual Inspection…  
**Command:** `Inspection_VisualInspection`

Visual Inspection is the primary tool of the Inspection workbench. It creates
an `Inspection::Feature` document object that stores the per-point deviation
distances between an actual geometry and one or more nominal geometries.

---

## Step-by-step

**Prerequisites:**

- At least one "actual" object in the document — a mesh, point cloud, or Part solid
  representing the real part.
- At least one "nominal" object — a mesh, point cloud, or Part solid
  representing the design.

1. Activate the **Inspection workbench**.
2. Go to **Inspection → Visual Inspection…**.
3. The Visual Inspection dialog opens with two lists:
   - **Actual** — all objects in the document that can serve as the scanned/manufactured geometry.
   - **Nominal** — all objects that can serve as the design reference.
4. In the **Actual** list, click the object that represents the manufactured part.
   Only one Actual object is allowed.
5. In the **Nominal** list, click one or more objects that represent the design intent.
   Multiple nominals are supported — the feature checks each actual point against
   all nominals and reports the minimum distance to any of them.
6. Under **Parameter**, set:
   - **Search distance** — the maximum distance (in mm) to search. Points on
     the actual geometry that lie farther than this from any nominal surface
     receive no result (displayed in grey). Default: 0.05 mm.
   - **Thickness** — a signed offset applied to the nominal before computing
     distances. Used to account for uniform material thickness (e.g. a sheet
     metal blank checked against its bent form). Default: 0.0 mm.
7. Click **OK**. FreeCAD creates an `Inspection::Feature` object in the
   document tree and populates it with a `Distances` list — one signed float
   per point on the actual geometry.
8. The 3-D view switches to show the actual geometry coloured by deviation
   (green = near zero, red = positive, blue = negative, grey = out of range).
   A colour bar appears in the 3-D view.

---

## Dialog reference

| Control | Description |
|---------|-------------|
| **Actual** list | Selectable list of document objects usable as actual geometry. Click once to select. |
| **Nominal** list | Selectable list of document objects usable as nominal geometry. Multiple selections allowed. |
| **Search distance** | Maximum distance (mm) within which a nearest-nominal point is searched. Points beyond this are marked as out-of-range. Default: 0.05 mm. |
| **Thickness** | Offset applied to the nominal surface before distance computation. Use for sheet metal or coating thickness. Default: 0.0 mm. |
| **OK** | Accepts settings, creates/updates the `Inspection::Feature`. |
| **Cancel** | Dismisses without changes. |

---

## The Inspection::Feature object

After clicking OK, the document tree gains an `Inspection::Feature` with
these properties:

| Property | Type | Description |
|----------|------|-------------|
| `Actual` | Link | Reference to the actual object |
| `Nominals` | LinkList | References to all nominal objects |
| `SearchRadius` | Float (mm) | The search distance at creation time |
| `Thickness` | Float (mm) | The thickness offset at creation time |
| `Distances` | PropertyDistanceList | Computed signed distances, one per actual point |

The `Distances` list maps 1-to-1 to the sampled points on the actual geometry.
For a mesh Actual, each entry corresponds to a mesh vertex. For a Part solid
Actual, the solid is tessellated internally and each tessellation point gets an
entry.

---

## Updating an inspection

The `Inspection::Feature` does **not** update automatically if the geometry
changes. To refresh:

1. Delete the existing `Inspection::Feature`.
2. Re-run **Inspection → Visual Inspection…** with the updated objects.

Alternatively, touch the feature via Python:

```python
doc = App.ActiveDocument
feat = doc.getObject("Feature")  # or whatever it is named
feat.touch()
doc.recompute()
```

---

## Adjusting the colour bar

The false-colour display uses a colour bar in the 3-D view:

- **Drag the colour bar ends** to change the min/max of the mapped range.
- Setting the range to ±0.1 mm, for example, highlights all deviations larger
  than 0.1 mm in saturated red or blue.
- The colour bar scale is purely visual — it does not affect the stored
  `Distances` values.

---

## Common mistakes and pitfalls

!!! warning "Search distance too small — all points shown in grey"
    **Cause:** The default Search Radius of 0.05 mm is very tight. For a
    coarse mesh or a part with known deviation larger than 0.05 mm, every point
    will be out-of-range and shown in grey.  
    **Fix:** Increase the Search distance to match the expected deviation range
    (e.g. 1.0–5.0 mm for first-article inspection of machined parts).

!!! warning "Actual and Nominal are swapped"
    **Cause:** The sign convention depends on which object is Actual. If you
    put the CAD model as Actual and the scan as Nominal, positive deviations
    will appear where the scan is inside the model (which is the opposite of
    the usual convention).  
    **Fix:** Always select the scanned/manufactured part as Actual and the
    CAD design as Nominal.

!!! warning "Only one Actual object is allowed"
    **Cause:** The feature stores one distance per actual point; multiple
    actual objects would require multiple features.  
    **Fix:** Merge multiple actual meshes into one before running inspection, or
    run one inspection per actual object.

!!! warning "Results do not update after geometry change"
    **Cause:** `Inspection::Feature` is not parametrically linked to its inputs.  
    **Fix:** Delete the feature and re-run Visual Inspection with the updated geometry.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
actual = doc.getObject("ScanMesh")    # Mesh::Feature or Points::Feature or Part::Feature
nominal = doc.getObject("DesignBody") # same choices

feat = doc.addObject("Inspection::Feature", "Inspection")
feat.Actual = actual
feat.Nominals = [nominal]
feat.SearchRadius = 1.0   # mm — search up to 1 mm from each actual point
feat.Thickness = 0.0

doc.recompute()

# Access the distances
distances = feat.Distances.getValues()   # list of floats; NaN/max = out of range
import statistics
valid = [d for d in distances if abs(d) < feat.SearchRadius]
print(f"Mean deviation: {statistics.mean(valid):.4f} mm")
print(f"Max deviation:  {max(valid):.4f} mm")
print(f"Min deviation:  {min(valid):.4f} mm")
```

---

## See also

- [Inspection Workbench](../index.md) — overview and workflow
- [Inspect Element](inspect-element.md) — hover tool to read per-point distances interactively
