# Measure

> **In one sentence:** The Measure tool opens a task panel where you select
> geometry and FreeCAD automatically picks the appropriate measurement type
> — distance, angle, length, area, radius, position, or centre of mass —
> and displays the annotated result in the 3-D view.

---

## Overview

**Workbench:** Measure (also accessible from any workbench via the Measure menu)  
**Menu:** Measure → Measure  
**Command:** `Std_Measure`

The Measure tool is FreeCAD's **Unified Measurement Facility**: a single
entry point for all geometry measurements. Rather than choosing separate
"Measure Distance" or "Measure Angle" tools, you pick geometry and the
tool selects the most appropriate measurement automatically.

---

## Measurement types

| Type | Triggered by | What is reported |
|------|-------------|-----------------|
| **Distance** | Two vertices, two faces, vertex + face, or edge + face | Shortest distance between the two shapes |
| **Distance Free** | Two free points (not on geometry) | Point-to-point distance in 3-D space |
| **Angle** | Two planar faces, two edges, or face + edge | Angle between the planes or tangents |
| **Length** | One edge or wire | Total arc length of the edge |
| **Radius** | One circular edge, arc, or cylindrical face | Radius (and optionally diameter) |
| **Area** | One face | Surface area |
| **Position** | One vertex | X, Y, Z coordinates |
| **Centre of Mass** | One solid or compound | X, Y, Z of the centroid |

FreeCAD selects the measurement type based on what is selected. If the
selection matches multiple types, the highest-priority match wins.

---

## Step-by-step

1. Activate **Measure → Measure** (or click the Measure button in the
   toolbar).
2. The Measure task panel opens in the left side panel.
3. Click one or more sub-elements in the 3-D view:
   - **One element:** position, length, area, or radius depending on type.
   - **Two elements:** distance or angle depending on types selected.
4. The result appears immediately in the task panel and as an annotated
   label in the 3-D view.
5. Optionally click **Save** to store the measurement as a persistent
   `Measure::MeasureDistance` (or similar) object in the document tree.
6. Click a new element to start a new measurement, or click **Clear** to
   reset.
7. Click **Close** (or press **Escape**) to exit the task panel.

---

## The task panel

| Element | Description |
|---------|-------------|
| Selection list | Shows the currently selected sub-elements |
| Measurement type | Detected automatically; shown as a label |
| Result | The numeric measurement value with units |
| **Save** button | Stores the result as a permanent document object |
| **Settings** | Gear icon — toggle auto-save and annotation display options |
| **Close** button | Closes the task panel without saving |

### Auto-save option

In **Settings → Auto Save**: when enabled, every measurement is saved to
the document automatically without clicking Save. Useful for recording a
series of measurements without interruption.

### New measurement behaviour

In **Settings → New measurement starts immediately**: when enabled, clicking
a new element while a measurement is displayed automatically starts a fresh
measurement. When disabled, you must click **Clear** first.

---

## Reading measurements in the 3-D view

Saved measurements appear as **annotation objects** in the 3-D view:

- A leader line connects the measured elements to the result label.
- The label shows the measurement value and unit.
- Clicking the annotation selects it; you can move the label by dragging.
- Delete the annotation by selecting it and pressing Delete.

---

## Measurement type details

### Distance

Measures the **minimum distance** between two shapes. For two faces, this
is the shortest line connecting any point on one face to any point on the
other face.

**Edge cases:**
- Two overlapping faces: distance = 0.
- Faces that are coplanar (same plane): distance = 0.
- Parallel faces at different heights: the perpendicular gap.
- Non-parallel faces: the shortest oblique distance.

### Distance Free

Measures the straight-line distance between two arbitrary points that are
**not required to be on existing geometry** — click anywhere in the 3-D
view to place both endpoints. Used for measuring along a known direction
when no vertex exists at the needed location.

### Angle

Measures the angle between:
- Two planar faces (dihedral angle).
- Two straight edges (angle at their intersection or between their directions).
- A face and a straight edge.

The result is in degrees, from 0° to 180°.

!!! note "Angle between faces is the dihedral angle"
    The dihedral angle is measured between the face normals, not the edges.
    For two perpendicular faces, the dihedral angle is 90°. For two faces
    forming a sharp 45° chamfer, the dihedral angle is 135° (supplement
    of the chamfer angle).

### Length

Measures the arc length of one edge. For a straight edge, this equals
the Euclidean distance between its endpoints. For a curved edge (arc,
B-spline), this is the integral of the curve length.

### Radius

Measures the radius of a circular edge, arc, or the cross-section radius
of a cylindrical face. The result shows the radius; the diameter (2 ×
radius) is usually shown alongside.

### Area

Measures the surface area of one face. For planar faces, this is the
exact 2-D area. For curved faces, this is the surface integral.

Units are in mm² by default.

### Position

Reports the X, Y, Z coordinates of a selected vertex in the document's
global coordinate system.

### Centre of Mass

Reports the X, Y, Z centroid of a solid or compound. This is the weighted
average position over the entire volume, assuming uniform density.

Activated via Python or from the MeasureCOM module:
```python
from MeasureCOM import makeMeasureCOM

doc = App.ActiveDocument
solid = doc.getObject("Body")
com = makeMeasureCOM(solid)
doc.recompute()
print(com.CenterOfMass)   # App.Vector(x, y, z)
```

---

## Python API

### Creating measurements programmatically

```python
import Measure

doc = App.ActiveDocument
body = doc.getObject("Body")

# Measure the distance between Face1 and Face3
dist = doc.addObject("Measure::MeasureDistance", "Dist1")
dist.Element1 = (body, ["Face1"])
dist.Element2 = (body, ["Face3"])
doc.recompute()
print(dist.Distance)   # result in mm
```

### Measurement object types

| Object type | Measured quantity |
|-------------|-----------------|
| `Measure::MeasureDistance` | Minimum distance |
| `Measure::MeasureAngle` | Angle between two shapes |
| `Measure::MeasureLength` | Edge arc length |
| `Measure::MeasureRadius` | Circle/cylinder radius |
| `Measure::MeasureArea` | Face surface area |
| `Measure::MeasurePosition` | Vertex coordinates |

---

## Common mistakes and pitfalls

!!! warning "Distance between two faces on the same solid may be zero"
    If two faces of the same solid share an edge or are coplanar, the
    minimum distance between them is zero. The measurement is geometrically
    correct — the faces are touching. To measure the gap between two separate
    solids, select faces from each.

!!! warning "Angle shows the dihedral angle, not the supplement"
    A 90° corner gives an angle of 90°. A 45° chamfer face (measured against
    the flat face it was cut from) gives 135° — the angle between face normals
    is the supplement of the visual corner angle. For the corner angle,
    subtract from 180°.

!!! warning "Length measures arc length, not chord length"
    For a curved edge, Length gives the arc length (the path distance along
    the curve), not the straight-line distance between the edge endpoints.
    For a straight edge, arc length = chord length.

!!! warning "Saved measurements do not update automatically"
    A saved Measure object records the selection at the time it was created.
    If you change the geometry (e.g. move a face), the measurement is NOT
    updated automatically. Delete the old measurement and create a new one.

---

## See also

- [Measure Workbench](../index.md) — Quick Measure and workbench overview
- TechDraw Dimension — linked dimensions for 2-D drawing sheets
- Sketcher Dimension — driving/reference constraints inside a sketch
