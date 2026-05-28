# Drafting Tools

> **In one sentence:** The Drafting menu contains all tools for creating
> 2-D geometry — lines, arcs, curves, rectangles, polygons, splines,
> Bézier curves, text outlines, hatching, and face binders.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting

| Tool | Description |
|------|-------------|
| [Line](#line) | Single straight segment |
| [Wire (Polyline)](#wire-polyline) | Connected multi-segment polyline |
| [Fillet](#fillet) | Fillet arc between two lines |
| [Arc](#arc) | Arc from centre, radius, angles |
| [Arc by 3 Points](#arc-by-3-points) | Arc through three points |
| [Circle](#circle) | Full circle |
| [Ellipse](#ellipse) | Ellipse or elliptical arc |
| [Rectangle](#rectangle) | Axis-aligned rectangle |
| [Polygon](#polygon) | Regular polygon |
| [B-Spline](#b-spline) | B-spline through control points |
| [Cubic Bézier Curve](#cubic-bezier-curve) | Degree-3 Bézier curve |
| [Bézier Curve](#bezier-curve) | Bézier curve of arbitrary degree |
| [Point](#point) | Single point vertex |
| [Facebinder](#facebinder) | Face from selected solid faces |
| [ShapeString](#shapestring) | Text as wire outlines |
| [Hatch](#hatch) | Hatch fill for closed shapes |

---

## Intuition

### Draft vs Sketcher for 2-D geometry

Draft and Sketcher both create 2-D geometry, but serve different purposes:

| Aspect | Draft | Sketcher |
|--------|-------|----------|
| Primary use | Freeform drawing, floor plans, annotations | Constrained profiles for Part Design features |
| Constraints | None — explicit coordinates and snapping | Geometric and dimensional constraints |
| Parametric | Objects store explicit coordinates (editable) | Objects store constraints (solver-driven) |
| Working plane | Any plane in 3-D space | Always the sketch plane |
| Output | Stand-alone 2-D objects or for TechDraw | Profiles for Pad, Pocket, Loft, etc. |

### Placement on the working plane

All Draft tools create geometry on the current **working plane**. Set it
before drawing with **Draft → Utilities → Select Working Plane** or by
holding a face in the 3-D view.

---

## Line

**Menu:** Draft → Drafting → Line  
**Command:** `Draft_Line`

Draws a single straight line segment between two picked or typed points.
The result is a `Draft::Wire` with two points and `Closed = False`.

### Step-by-step

1. Activate **Draft → Drafting → Line**.
2. Pick the start point in the 3-D view or type X, Y, Z coordinates in
   the input boxes.
3. Pick the end point.
4. Press **Enter** or click **Close** to finish.

### Key properties

| Property | Description |
|----------|-------------|
| Start / End | Explicit coordinates of the endpoints |
| Line Color | RGB line colour |
| Line Width | Display line width |
| Subdivision Type | None / Linear subdivisions |

### Python API

```python
import Draft

line = Draft.make_line(
    App.Vector(0, 0, 0),   # start
    App.Vector(100, 0, 0)  # end
)
App.ActiveDocument.recompute()
```

---

## Wire (Polyline)

**Menu:** Draft → Drafting → Wire  
**Command:** `Draft_Wire`

Draws a connected sequence of straight line segments. Each click adds a
vertex. The polyline can be left open or closed into a face.

### Step-by-step

1. Activate **Draft → Drafting → Wire**.
2. Pick points one by one to add segments.
3. Press **A** to toggle **Relative mode** (each point offset from previous).
4. Press **C** to close the wire back to the first point.
5. Press **Enter** or double-click the last point to finish open.

### Wire properties

| Property | Description |
|----------|-------------|
| Points | List of all vertices |
| Closed | Whether the last vertex connects to the first |
| Make Face | Create a filled face when Closed is True |
| Fillet Radius | Optional radius to fillet all corners |
| Chamfer Size | Optional chamfer on all corners |

### Python API

```python
import Draft

pts = [
    App.Vector(0, 0, 0),
    App.Vector(50, 0, 0),
    App.Vector(50, 50, 0),
    App.Vector(0, 50, 0),
]
wire = Draft.make_wire(pts, closed=True, face=True)
App.ActiveDocument.recompute()
```

---

## Fillet

**Menu:** Draft → Drafting → Fillet  
**Command:** `Draft_Fillet`

Creates a fillet arc between two existing Draft lines. The lines must share
or nearly share an endpoint (the corner to be filleted).

### Parameters

| Parameter | Description |
|-----------|-------------|
| Radius | Radius of the fillet arc |
| Delete original wires | Remove the original lines after filleting |

Fillet joins two lines with a tangent arc of the specified radius. If the
lines are too short for the given radius, the fillet is not created.

---

## Arc

**Menu:** Draft → Drafting → Arc Tools → Arc  
**Command:** `Draft_Arc`

Draws a circular arc by specifying centre point, radius, start angle, and
aperture angle. This is the classic three-step arc construction.

### Step-by-step

1. Activate **Draft → Drafting → Arc Tools → Arc**.
2. Pick the **centre point**.
3. Move the cursor to set the **radius** (or type a value).
4. Click to set the **start angle** on the circle.
5. Click to set the **end angle** (the aperture).

### Key properties

| Property | Description |
|----------|-------------|
| Radius | Circle radius |
| First Angle | Start angle (degrees, measured from X-axis) |
| Last Angle | End angle — arc sweeps from First to Last |

### Python API

```python
import Draft

arc = Draft.make_circle(
    radius=30,
    face=False,
    startangle=0,
    endangle=90,
    placement=App.Placement(
        App.Vector(0, 0, 0), App.Rotation()
    )
)
App.ActiveDocument.recompute()
```

---

## Arc by 3 Points

**Menu:** Draft → Drafting → Arc Tools → Arc by 3 Points  
**Command:** `Draft_Arc_3Points`

Draws a circular arc through three clicked points. FreeCAD computes the
unique circle passing through all three points and shows the arc between
the first and third points.

This is often faster than the centre-radius-angle method when you know
where the arc must pass (e.g. fitting an arc through three survey points).

---

## Circle

**Menu:** Draft → Drafting → Circle  
**Command:** `Draft_Circle`

Draws a full circle from a centre point and radius.

### Key properties

| Property | Description |
|----------|-------------|
| Radius | Circle radius |
| Make Face | Fill the circle with a face (disc) |
| First Angle | Set to create an arc instead of a full circle |
| Last Angle | End of arc if First Angle is set |

### Python API

```python
import Draft

circle = Draft.make_circle(radius=25, face=True)
circle.Placement.Base = App.Vector(0, 0, 0)
App.ActiveDocument.recompute()
```

---

## Ellipse

**Menu:** Draft → Drafting → Ellipse  
**Command:** `Draft_Ellipse`

Draws an ellipse or elliptical arc by picking two diagonal corners of its
bounding rectangle. The semi-major and semi-minor axes are derived from
the bounding box.

### Key properties

| Property | Description |
|----------|-------------|
| MajorRadius | Semi-major axis length |
| MinorRadius | Semi-minor axis length |
| Make Face | Fill the ellipse with a face |
| First Angle / Last Angle | If set, draws an elliptical arc |

### Python API

```python
import Draft

ellipse = Draft.make_ellipse(
    majradius=50,
    minradius=25,
    face=True
)
App.ActiveDocument.recompute()
```

---

## Rectangle

**Menu:** Draft → Drafting → Rectangle  
**Command:** `Draft_Rectangle`

Draws an axis-aligned rectangle by picking two diagonal corner points. The
edges are always parallel to the working plane axes.

### Key properties

| Property | Description |
|----------|-------------|
| Length | Width of the rectangle (X direction) |
| Height | Height of the rectangle (Y direction) |
| Make Face | Fill with a face |
| Fillet Radius | Round all four corners |
| Chamfer Size | Chamfer all four corners |
| Rows / Columns | Subdivide into a grid of sub-faces |

### Python API

```python
import Draft

rect = Draft.make_rectangle(
    length=100,   # width
    height=60,    # height
    placement=App.Placement(),
    face=True
)
App.ActiveDocument.recompute()
```

---

## Polygon

**Menu:** Draft → Drafting → Polygon  
**Command:** `Draft_Polygon`

Draws a regular polygon (all sides equal, all angles equal) by specifying
the number of sides and the circumscribed or inscribed radius.

### Parameters

| Property | Description |
|----------|-------------|
| FacesNumber | Number of sides (3 = triangle, 6 = hexagon, etc.) |
| Radius | Circumradius (centre to vertex) |
| Draw Mode | Inscribed (vertex on radius) vs Circumscribed (edge tangent to radius) |
| Make Face | Fill with a face |

### Python API

```python
import Draft

hex = Draft.make_polygon(
    nfaces=6,
    radius=30,
    inscribed=True,
    face=True
)
App.ActiveDocument.recompute()
```

---

## B-Spline

**Menu:** Draft → Drafting → B-Spline  
**Command:** `Draft_BSpline`

Draws a B-spline (non-rational) through a set of control points. Each
click adds a control point; the curve passes through or near each point
depending on the degree.

### Key properties

| Property | Description |
|----------|-------------|
| Points | Control point list |
| Closed | Close the spline back to the first point |
| Make Face | Fill a closed spline with a face |
| Parameterization | Controls how knots are spaced |

B-splines in Draft are uniform and interpolate through the control points.
For full NURBS control (weights, knots), use Part workbench NURBS tools.

---

## Cubic Bézier Curve

**Menu:** Draft → Drafting → Bézier Tools → Cubic Bézier Curve  
**Command:** `Draft_CubicBezCurve`

Draws a cubic (degree-3) Bézier curve. Each segment is defined by four
control points: two endpoints and two handles. The handles are shown as
small blue circles that can be dragged.

Click to place points alternating endpoint → handle → handle → endpoint.
The result is a smooth curve whose tangent at each endpoint is controlled
by the adjacent handle.

---

## Bézier Curve

**Menu:** Draft → Drafting → Bézier Tools → Bézier Curve  
**Command:** `Draft_BezCurve`

Draws a Bézier curve of arbitrary degree. Each click adds another control
point; the curve blends through all points using a single polynomial of
degree = n − 1.

For a 3-point Bézier: quadratic (parabola).  
For a 4-point Bézier: cubic.  
For n points: degree n − 1.

High-degree Béziers become impractical for large point counts — use
B-splines for complex curves with many points.

---

## Point

**Menu:** Draft → Drafting → Point  
**Command:** `Draft_Point`

Places a single point vertex object at a picked or typed position. Draft
points are used as construction references, for Point Arrays, or as
measurement markers.

### Python API

```python
import Draft

pt = Draft.make_point(
    X=10, Y=20, Z=0,
    color=(1, 0, 0),
    name="MyPoint",
    point_size=5
)
App.ActiveDocument.recompute()
```

---

## Facebinder

**Menu:** Draft → Drafting → Facebinder  
**Command:** `Draft_Facebinder`

Creates a compound object made from selected faces of existing solid
objects. The facebinder follows the faces as the solid changes shape
(parametric). It is useful for creating flat reference surfaces from
solid faces, or for applying a different material to a specific face.

Select one or more faces (hold Ctrl to multi-select) before activating
the tool.

---

## ShapeString

**Menu:** Draft → Drafting → ShapeString  
**Command:** `Draft_ShapeString`

Creates a compound of closed wire outlines from a text string using a
font file (TrueType or OpenType). The result is solid-quality geometry
that can be extruded with Part Design Pad or Part Extrude to make
3-D lettering.

### Parameters

| Parameter | Description |
|-----------|-------------|
| String | The text to convert |
| Font file | Path to a .ttf or .otf font file |
| Size | Character height (in mm, model units) |
| Justification | Left / Right / Centre alignment |

### Workflow: 3-D text

```
1. Place ShapeString on the working plane.
2. Switch to Part workbench.
3. Select the ShapeString, then Part → Extrude.
4. Set direction and depth to create 3-D letters.
```

### Python API

```python
import Draft

ss = Draft.make_shapestring(
    String="FreeCAD",
    FontFile="/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf",
    Size=10.0,
    Tracking=0
)
ss.Placement.Base = App.Vector(0, 0, 0)
App.ActiveDocument.recompute()
```

---

## Hatch

**Menu:** Draft → Drafting → Hatch  
**Command:** `Draft_Hatch`

Fills a closed Draft shape (wire, rectangle, circle, polygon) with a hatch
pattern. Supported pattern sources:

- **SVG patterns** — referenced from an SVG file's `<pattern>` elements.
- **PAT patterns** — AutoCAD-format .PAT hatch definition files.

FreeCAD ships with a set of bundled SVG patterns (available in the file
selector dropdown). Custom `.pat` files can be loaded from disk.

### Parameters

| Parameter | Description |
|-----------|-------------|
| File | SVG or PAT file containing the pattern |
| Pattern | Name of the pattern within the file |
| Scale | Scale factor for the pattern spacing |
| Rotation | Rotation angle of the pattern (degrees) |

### Notes

- The shape must be a **closed** wire or face.
- Hatch is display-only — it has no solid geometry.
- For TechDraw output, use TechDraw's built-in Hatch tools instead.

---

## Common mistakes and pitfalls

!!! warning "Objects placed at Z ≠ 0 when working plane is wrong"
    If you draw an object and it appears at an unexpected height or
    orientation, the working plane is not what you think. Check the working
    plane indicator in the Draft toolbar and reset with **Select Working Plane**.

!!! warning "ShapeString requires an absolute font path"
    The font file path in ShapeString is stored as an absolute path. If you
    move the document to a different machine, the font reference will break.
    Use a font available on both machines, or embed a copy.

!!! warning "Wire Make Face requires a coplanar closed wire"
    If Make Face is True but the wire is not closed or its vertices are not
    coplanar, FreeCAD cannot compute a face. Check the Closed property and
    verify all points have the same Z value.

!!! warning "B-Spline degree increases with point count"
    Draft B-Spline uses a uniform B-spline that interpolates all control
    points. Adding many points raises the effective curve degree. For
    controlled smooth curves, use fewer points and the Cubic Bézier tool.

---

## See also

- [Annotation Tools](annotation.md) — text, dimensions, and labels
- [Modification Tools](modification.md) — move, rotate, stretch, convert
- [Snapping](snapping.md) — snap modes for precise placement
