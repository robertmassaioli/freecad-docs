# Dimensions

> **In one sentence:** TechDraw dimensions are linked measurements placed on
> views — they read from the 3-D model geometry so they update automatically
> when the model changes, and they control how tolerances and prefixes are
> displayed.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Dimensions

| Tool | Description |
|------|-------------|
| [Insert Dimension](#insert-dimension) | Smart dimension — picks type from selection |
| [Insert Length Dimension](#insert-length-dimension) | Straight distance between two points |
| [Insert Horizontal Dimension](#insert-horizontal-dimension) | Horizontal distance |
| [Insert Vertical Dimension](#insert-vertical-dimension) | Vertical distance |
| [Insert Radius Dimension](#insert-radius-dimension) | Radius of an arc or circle |
| [Insert Diameter Dimension](#insert-diameter-dimension) | Diameter of a circle |
| [Insert Angle Dimension](#insert-angle-dimension) | Angle between two edges |
| [Insert 3-Point Angle Dimension](#insert-3-point-angle-dimension) | Angle via three vertices |
| [Insert Area Annotation](#insert-area-annotation) | Area of a face |
| [Insert Horizontal Extent Dimension](#insert-horizontal-extent-dimension) | Total horizontal span |
| [Insert Vertical Extent Dimension](#insert-vertical-extent-dimension) | Total vertical span |
| [Repair Dimension References](#repair-dimension-references) | Re-link broken dimensions |
| [Insert Balloon Annotation](#insert-balloon-annotation) | Balloon callout with leader |
| [Insert Axonometric Length Dimension](#insert-axonometric-length-dimension) | True length on isometric view |

---

## Intuition

### Projected vs true dimensions

A TechDraw view is a 2-D projection. The *apparent* length of an edge in
the view depends on the projection angle. For example, a 100 mm horizontal
edge viewed at an angle appears shorter than 100 mm in the drawing.

TechDraw handles this with two dimension modes:

- **Projected dimension** — measures the apparent 2-D length in the view.
  Fast to create, but only correct for edges that are perfectly parallel
  to the drawing plane.
- **3-D referenced dimension** — the dimension is linked to the actual 3-D
  geometry and reports the true length regardless of view angle. This is
  the preferred mode for precision drawings.

When you pick edges that TechDraw can trace back to 3-D geometry, it
automatically creates a 3-D referenced dimension. The dimension turns
orange-yellow in colour if the reference is broken (e.g. after a model
topology change).

### Dimension properties

Every dimension has a rich set of properties accessible in the Data and
View panels:

**Data (what the dimension measures):**

| Property | Description |
|----------|-------------|
| Type | LengthX, LengthY, Length, Radius, Diameter, Angle, etc. |
| References 2D | The view edges / vertices the dimension points to |
| References 3D | The 3-D edges / vertices (for true 3-D dimensions) |
| Arbitrary | Override the displayed value with custom text |
| FormatSpec | printf-style format string (`%.2f` = 2 decimal places) |
| OverTolerance | Upper tolerance value |
| UnderTolerance | Lower tolerance value |
| EqualTolerance | Symmetric tolerance (± value) |

**View (how the dimension looks):**

| Property | Description |
|----------|-------------|
| Font | Dimension text font |
| FontSize | Text height |
| ArrowStyle | Arrowhead style (open, filled, tick, dot, etc.) |
| ArrowSize | Arrowhead size |
| LineWidth | Extension and dimension line weight |
| FlipArrowheads | Move arrowheads outside the extension lines |
| Color | Dimension line colour |
| GapFactorISO | Gap between extension line start and geometry |

---

## Insert Dimension

**Menu:** TechDraw → Dimensions → Insert Dimension

The smart dimension tool. Select geometry in a view and Insert Dimension
chooses the most appropriate dimension type automatically:

- Two vertices → Length Dimension
- One edge → Length / Radius / Diameter (depending on edge type)
- Two edges → Angle or Length

This is the quickest way to dimension a drawing when you do not need to
specify the type explicitly.

### Step-by-step

1. Click a vertex, edge, or pair of geometry in a view.
2. Choose **TechDraw → Dimensions → Insert Dimension**.
3. The dimension appears on the view. Click to place the dimension text
   in a comfortable position.
4. The dimension is now live — it updates when the model changes.

---

## Insert Length Dimension

**Menu:** TechDraw → Dimensions → Insert Length Dimension

Measures the straight-line distance between two selected points or along
a selected edge. The measurement is the true 3-D distance if geometry
references are available, or the projected 2-D distance otherwise.

### Step-by-step

1. Click two **vertices** in a view (hold Ctrl to select the second).
2. Choose **TechDraw → Dimensions → Insert Length Dimension**.
3. Click to place the dimension text.

### Tolerances

Set tolerance values in the dimension's **Data** properties:

- `OverTolerance` / `UnderTolerance` for asymmetric (e.g. +0.05 / -0.00).
- `EqualTolerance` for symmetric (e.g. ±0.05). Set `EqualTolerance = true`
  and `OverTolerance = 0.05`.

### Python API

```python
import FreeCAD as App
import TechDraw

doc = App.ActiveDocument
page = doc.getObject("Page")
view = doc.getObject("FrontView")

dim = doc.addObject("TechDraw::DrawViewDimension", "Length1")
dim.Type = "Distance"
dim.References2D = [(view, "Edge1")]
page.addView(dim)
doc.recompute()
```

---

## Insert Horizontal Dimension

**Menu:** TechDraw → Dimensions → Insert Horizontal Dimension

Measures only the **horizontal component** of the distance between two points,
regardless of the actual edge angle. The dimension line is always drawn
horizontally on the page.

Use this when the drawing standard requires the horizontal extent to be
called out separately from the diagonal length (e.g. horizontal position
of a hole centre from a datum edge).

---

## Insert Vertical Dimension

**Menu:** TechDraw → Dimensions → Insert Vertical Dimension

Measures only the **vertical component** of the distance between two points.
The dimension line is always drawn vertically on the page.

---

## Insert Radius Dimension

**Menu:** TechDraw → Dimensions → Insert Radius Dimension

Measures the radius of an arc or circle. The dimension line starts at the
arc centre and ends at the arc with an arrowhead pointing outward. The value
is prefixed with **R**.

### Step-by-step

1. Click a **circular arc or full circle** edge in a view.
2. Choose **TechDraw → Dimensions → Insert Radius Dimension**.
3. Click to place the dimension text.

---

## Insert Diameter Dimension

**Menu:** TechDraw → Dimensions → Insert Diameter Dimension

Measures the diameter of a circle. The dimension line passes through the
circle centre with arrowheads at both ends. The value is prefixed with **⌀**
(the diameter symbol).

### Step-by-step

1. Click a **full circle** edge in a view.
2. Choose **TechDraw → Dimensions → Insert Diameter Dimension**.
3. Click to place the dimension text.

### Displaying on a radius instead

If the circle is small, showing the full diameter dimension through the
centre may be cramped. Set `FlipArrowheads = true` to move the arrowheads
outside the circle, or switch to a Radius Dimension.

---

## Insert Angle Dimension

**Menu:** TechDraw → Dimensions → Insert Angle Dimension

Measures the angle between two selected straight edges. The angle is the
interior angle at the intersection of the two lines.

### Step-by-step

1. Click the first **straight edge** in a view.
2. Ctrl-click the second **straight edge**.
3. Choose **TechDraw → Dimensions → Insert Angle Dimension**.
4. Click to place the dimension arc and text.

---

## Insert 3-Point Angle Dimension

**Menu:** TechDraw → Dimensions → Insert 3-Point Angle Dimension

Measures the angle at a vertex defined by three selected vertices: the angle
is measured from the line (vertex1 → vertex2) to the line (vertex2 → vertex3),
where vertex2 is the apex.

Use this for angles that cannot be defined by two edges alone (e.g. the
angle of a hole pattern measured from a central point).

---

## Insert Area Annotation

**Menu:** TechDraw → Dimensions → Insert Area Annotation

Calculates and displays the area of a selected face in the view. The value
appears as a text annotation (not a dimension line) placed on the face.

Units follow the dimension unit settings (mm² by default).

---

## Insert Horizontal Extent Dimension

**Menu:** TechDraw → Dimensions → Insert Horizontal Extent Dimension

Measures the total horizontal span of all selected edges or the entire view.
Creates a dimension from the leftmost to the rightmost point of the
selection, with the dimension line placed below (or above) the view.

Use for overall width callouts on complex profiles where no single edge
spans the full width.

---

## Insert Vertical Extent Dimension

**Menu:** TechDraw → Dimensions → Insert Vertical Extent Dimension

Measures the total vertical span of all selected edges or the entire view.
Creates a dimension from the bottommost to the topmost point.

---

## Repair Dimension References

**Menu:** TechDraw → Dimensions → Repair Dimension References

When a model undergoes topology changes (a face is split, a feature is
deleted and re-added, or a body is Boolean-combined), the internal edge and
vertex IDs can change. Dimensions that referenced the old IDs turn orange
and display `?` instead of a value.

Repair Dimension References opens a dialog that lets you re-select the
correct geometry for each broken dimension.

### Preventing broken dimensions

- Avoid renaming or deleting features that dimensions reference.
- Use **Part Design → Named constraints** (named sketch dimensions) as
  reference geometry — named geometry is more stable across topology changes
  than raw edge/vertex IDs.
- After significant model restructuring, run Repair Dimension References
  before delivering the drawing.

---

## Insert Balloon Annotation

**Menu:** TechDraw → Dimensions → Insert Balloon Annotation

Places a balloon (a circled number or letter with a leader line) on the
view. Balloons are used in assembly drawings to call out part numbers that
correspond to a Bill of Materials table.

### Step-by-step

1. Choose **TechDraw → Dimensions → Insert Balloon Annotation**.
2. Click the point on the view to attach the leader tip.
3. Click again to place the balloon body.
4. Double-click the balloon to edit its text.

### Properties

| Property | Description |
|----------|-------------|
| Text | The text inside the balloon |
| BubbleShape | Circle, Rectangle, Triangle, Hexagon, etc. |
| EndType | Leader arrow style |
| KinkLength | Length of the horizontal kink segment |

---

## Insert Axonometric Length Dimension

**Menu:** TechDraw → Dimensions → Insert Axonometric Length Dimension

Places a length dimension on an isometric or other axonometric view that
displays the **true 3-D length** of a selected edge, not the foreshortened
projected length. The dimension line is drawn parallel to the edge on the
isometric view.

Without this tool, a standard Length Dimension on an isometric view would
read the foreshortened projected length, which is incorrect for engineering
communication.

### Step-by-step

1. Select a straight edge in an axonometric (isometric) view.
2. Choose **TechDraw → Dimensions → Insert Axonometric Length Dimension**.
3. The dimension line appears parallel to the edge with the true length value.

---

## Common mistakes and pitfalls

!!! warning "Orange / yellow dimensions indicate broken references"
    Immediately after a model change, check whether any dimensions have
    turned orange. Orange means the referenced geometry no longer exists
    or has changed ID. Use Repair Dimension References before delivering
    the drawing.

!!! warning "Projected dimensions on angled edges are misleading"
    A standard Length Dimension on an edge that is not parallel to the
    drawing plane measures the projected (foreshortened) length. This is
    geometrically incorrect and will confuse the machinist. Select 3-D
    vertices or use 3-D referenced dimensions for angled edges.

!!! warning "Diameter symbol on radius dimensions"
    Insert Diameter Dimension adds the ⌀ prefix automatically. If you
    use Insert Length Dimension on a circle diameter, you will get a bare
    number without the prefix — add it manually via `FormatSpec = "⌀%.2f"`.

!!! warning "Tolerance display requires FormatSpec adjustment"
    Tolerance values are shown only when `OverTolerance` / `UnderTolerance`
    are non-zero. The format string must also include the tolerance specifier
    `%+.2f` or the tolerance won't appear even if the values are set.

---

## See also

- [Views](views.md) — create the views that dimensions are placed on
- Annotations — text blocks, leader lines, and balloons (covered in this page)
- Extensions — Dimension Format/Organise — chain dimensions, coordinate dimensions, prefix tools
