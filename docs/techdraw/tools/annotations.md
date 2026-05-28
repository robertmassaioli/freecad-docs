# Annotations, Hatching, and Cosmetic Geometry

> **In one sentence:** Annotations add explanatory text, leader lines, and
> hatching to a drawing; cosmetic geometry adds construction lines and
> vertices that exist only on the drawing — not in the 3-D model.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Annotations / Add Lines / Add Vertices / Hatching

| Group | Tools |
|-------|-------|
| [Annotations](#annotations) | Text, rich text, leader line |
| [Hatching](#hatching) | SVG pattern hatch, geometric PAT hatch |
| [Cosmetic Lines](#cosmetic-lines) | Face centerline, 2-line/2-point centerline, cosmetic line, decorate/show/erase |
| [Cosmetic Vertices](#cosmetic-vertices) | Cosmetic vertex, midpoints, quadrants |
| [Stacking](#stacking) | Stack top/bottom/up/down |

---

## Intuition

### Annotations vs dimensions

**Dimensions** are linked to geometry and report a measurement. **Annotations**
are free-form text — general notes, reference standards, material callouts,
surface finish notes written as text rather than as ISO symbols.

### Cosmetic geometry

Cosmetic lines and vertices exist **only on the drawing**, not in the 3-D
model. They are used for:

- **Centerlines** — the axis of a hole, shaft, or boss drawn as a long-dash
  center line style.
- **Construction geometry** — reference lines for alignment or layout that
  are part of the drawing communication but are not model edges.
- **Pitch circle diameters** — the imaginary circle on which bolt holes sit.

Cosmetic geometry is styled with TechDraw's line types (center, hidden,
phantom, etc.) and does not affect the 3-D model in any way.

---

## Annotations

### Insert Annotation

**Menu:** TechDraw → Annotations → Insert Annotation

Places a plain multi-line text block on the drawing page. The text is
free-positioned and does not have a leader line.

#### Step-by-step

1. Choose **TechDraw → Annotations → Insert Annotation**.
2. The annotation appears on the page. Double-click it to edit the text.
3. Drag it to position.

#### Properties

| Property | Description |
|----------|-------------|
| Text | The annotation text (multi-line supported) |
| Font | Font family |
| TextSize | Text height in mm |
| TextColor | Text colour |
| MaxWidth | Width at which text wraps to the next line |

#### Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
page = doc.getObject("Page")

anno = doc.addObject("TechDraw::DrawViewAnnotation", "Note1")
anno.Text = ["GENERAL TOLERANCE ISO 2768-m", "UNLESS OTHERWISE STATED"]
anno.TextSize = 3.5
page.addView(anno)
doc.recompute()
```

---

### Insert Rich Text Annotation

**Menu:** TechDraw → Annotations → Insert Rich Text Annotation

Places a text block that supports basic HTML-like formatting — bold, italic,
underline, font size changes within the same block. Useful for general notes
where you need to emphasise certain words or headings.

The rich text editor opens when you double-click the annotation after placing
it.

---

### Insert Leader Line

**Menu:** TechDraw → Annotations → Insert Leader Line

Places an annotation leader: a line with an arrowhead that points at
geometry and a text block at the tail. Leaders are used for:

- Referencing a specific feature ("DRILL AND TAP M6 × 1.0 DEEP 15")
- Pointing to a surface ("GRIND TO Ra 0.8")
- Any note that needs to be spatially linked to a specific location on
  the drawing

#### Step-by-step

1. Choose **TechDraw → Annotations → Insert Leader Line**.
2. Click the point on the view where the arrowhead tip should land.
3. Click one or more kink points to route the leader line.
4. Right-click to end the line and place the text.
5. Double-click the text box to edit the leader text.

#### Properties

| Property | Description |
|----------|-------------|
| Points | The list of vertices defining the leader path |
| EndType | Arrowhead style at the tip |
| StartType | End style at the text end (none, filled dot, open arrow) |
| Text | Optional text at the tail of the leader |
| LeaderParent | The view the leader is attached to |

---

## Hatching

### Hatch a Face

**Menu:** TechDraw → Hatching → Hatch a Face

Fills a closed face in a view with a repeating SVG pattern. This is the
standard way to draw section hatching (cross-hatching on cut surfaces) and
material-specific hatch patterns.

#### Step-by-step

1. Click a closed face in a view (it highlights).
2. Choose **TechDraw → Hatching → Hatch a Face**.
3. In the task panel, select an SVG hatch pattern file.
4. Set the scale and rotation of the pattern.
5. Click **OK**.

#### Bundled SVG hatch patterns

FreeCAD ships standard hatch patterns in:

```
<FreeCAD install>/Mod/TechDraw/Patterns/
```

Common patterns:

| File | Pattern |
|------|---------|
| `simple.svg` | Simple diagonal lines (ISO general hatching) |
| `steel.svg` | Steel (diagonal, 45°) |
| `aluminium.svg` | Aluminium (two sets of lines) |
| `wood.svg` | Wood grain |
| `concrete.svg` | Concrete (dot and dash) |

#### Properties

| Property | Description |
|----------|-------------|
| HatchPattern | Path to the SVG pattern file |
| HatchScale | Scale factor for the pattern repeat |
| HatchRotation | Rotation of the pattern in degrees |
| HatchOffset | Offset of the pattern origin |
| HatchColor | Override the pattern colour |

#### Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
page = doc.getObject("Page")
view = doc.getObject("SectionView")

hatch = doc.addObject("TechDraw::DrawHatch", "Hatch1")
hatch.Source = (view, ["Face1"])
hatch.HatchPattern = App.getResourceDir() + "Mod/TechDraw/Patterns/simple.svg"
hatch.HatchScale = 10.0
page.addView(hatch)
doc.recompute()
```

---

### Apply Geometric Hatch

**Menu:** TechDraw → Hatching → Apply Geometric Hatch

Fills a face using a **PAT file** — the industry-standard hatch definition
format used by AutoCAD and compatible CAD systems. PAT files define hatch
patterns as line families with angle, spacing, and dash parameters.

Use Geometric Hatch when you need:
- Exact AutoCAD-compatible hatch patterns (ANSI31, ISO02W100, etc.)
- Patterns defined by your company's CAD standard as a `.pat` file.

FreeCAD ships `PAT` files in:

```
<FreeCAD install>/Mod/TechDraw/PAT/
```

#### Step-by-step

1. Click a face in a view.
2. Choose **TechDraw → Hatching → Apply Geometric Hatch**.
3. Select the PAT file and the pattern name within it.
4. Set scale and rotation.
5. Click **OK**.

---

## Cosmetic Lines

### Add Face Centerline

**Menu:** TechDraw → Add Lines → Add Face Centerline

Draws a centerline (long-dash center line ISO style) through the centre of
a selected face. Most commonly used to mark the axis of a cylindrical face
— the shaft axis in a side view, or the bore axis in a section view.

#### Step-by-step

1. Click a face in a view (the cylindrical or flat face to centre).
2. Choose **TechDraw → Add Lines → Add Face Centerline**.
3. In the task panel, set the orientation (horizontal, vertical, or aligned
   to the face axis).
4. Set the extension length (how far the centerline extends beyond the face
   edges).
5. Click **OK**.

---

### Add Centerline Between 2 Lines

**Menu:** TechDraw → Add Lines → Add Centerline Between 2 Lines

Draws a centerline equidistant between two selected straight edges. Used
for:
- Marking the symmetry axis of a symmetric feature.
- Drawing the mid-plane between two parallel faces.

#### Step-by-step

1. Ctrl-click two straight edges in a view.
2. Choose **TechDraw → Add Lines → Add Centerline Between 2 Lines**.
3. Set extension and flip options.
4. Click **OK**.

---

### Add Centerline Between 2 Points

**Menu:** TechDraw → Add Lines → Add Centerline Between 2 Points

Draws a centerline between two selected vertices. Used when the centerline
axis is defined by two geometric points rather than by two edges or a face.

---

### Add Cosmetic Line Through 2 Points

**Menu:** TechDraw → Add Lines → Add Cosmetic Line Through 2 Points

Draws a straight line between two selected vertices using a chosen line
style. Unlike a centerline, the style is not automatically set to the
center-line dash pattern — you choose any available line type (solid,
dashed, dotted, phantom, etc.).

Use for pitch circle diameters, reference construction lines, or any line
that must appear on the drawing but is not a model edge.

---

### Change Appearance of Lines

**Menu:** TechDraw → Add Lines → Change Appearance of Lines

Changes the style, width, and colour of selected cosmetic lines or view
edges. Opens a dialog with line style (solid, dashed, center, phantom, etc.),
line width, and colour pickers.

---

### Show / Hide Invisible Edges

**Menu:** TechDraw → Add Lines → Show / Hide Invisible Edges

Toggles the visibility of individual hidden edges in a view. By default,
hidden edges can be shown as dashed lines if **ShowHidden** is true on the
view. This tool lets you selectively show or hide specific hidden edges
independent of the view-wide setting.

---

### Remove Cosmetic Objects

**Menu:** TechDraw → Remove Cosmetic Objects

Deletes selected cosmetic vertices, cosmetic lines, or centerlines from a
view. Select the cosmetic objects in the view before running this tool.

---

## Cosmetic Vertices

### Add Cosmetic Vertex

**Menu:** TechDraw → Add Vertices → Add Cosmetic Vertex

Adds a construction point (a small cross or dot) on a view at a position
you click. Cosmetic vertices can be used as snap targets for dimensions and
cosmetic lines.

Common uses:
- Marking the centre of a circle for a Radius Dimension where no vertex
  exists at the centre.
- Marking a specific point for a Length Dimension that has no underlying
  vertex.

---

### Add Midpoint Vertices

**Menu:** TechDraw → Add Vertices → Add Midpoint Vertices

Adds cosmetic vertices at the midpoints of selected edges. Useful for:
- Dimensioning to the midpoint of an edge.
- Placing a leader at the centre of a face edge.

#### Step-by-step

1. Click one or more edges in a view.
2. Choose **TechDraw → Add Vertices → Add Midpoint Vertices**.
3. A cosmetic vertex appears at the midpoint of each selected edge.

---

### Add Quadrant Vertices

**Menu:** TechDraw → Add Vertices → Add Quadrant Vertices

Adds cosmetic vertices at the 0°, 90°, 180°, and 270° positions on selected
circles or arcs. Useful for:
- Dimensioning to the topmost or rightmost point of a hole.
- Placing a Radius Dimension start point at the exact edge of a circle.

---

## Stacking

When views or annotations overlap, the **Stacking** tools control which
object appears on top.

| Tool | Description |
|------|-------------|
| Move View to Top of Stack | Render this view on top of all overlapping views |
| Move View to Bottom of Stack | Render beneath all overlapping views |
| Move View Up One Level | Move up one position in the z-order |
| Move View Down One Level | Move down one position |

**Menu:** TechDraw → Stacking → Stack Top / Stack Bottom / Stack Up / Stack Down

Select the view or annotation on the page, then apply the desired stacking
command. Stacking does not affect the model — it only controls page rendering
order.

---

## Common mistakes and pitfalls

!!! warning "Cosmetic lines are not model edges"
    Dimensions cannot be linked to cosmetic lines in the same way as model
    edges. Cosmetic lines are drawing-only objects. To dimension to a cosmetic
    position, add a Cosmetic Vertex and dimension to that vertex.

!!! warning "Centerlines need extension adjustment"
    The default extension length (how far a centerline extends beyond the
    geometry) is set in preferences. If centerlines look too short or too
    long for your standard, adjust the extension in the task panel or change
    the default in Edit → Preferences → TechDraw → Annotation.

!!! warning "Hatch pattern file paths are absolute"
    If you move the `.FCStd` file to another computer, hatch patterns stored
    with absolute paths may not be found. Use patterns from the bundled FreeCAD
    Patterns directory (accessed via relative App.getResourceDir()) rather than
    patterns stored in arbitrary locations.

!!! warning "Rich text annotation HTML is limited"
    The Rich Text Annotation supports only a subset of HTML — basic tags like
    `<b>`, `<i>`, `<u>`, `<font size>`. Full CSS or complex HTML is not
    supported. For complex layouts, use multiple annotations or an SVG symbol.

---

## See also

- [Views](views.md) — the views that annotations and hatching are placed on
- [Dimensions](dimensions.md) — linked measurements (distinct from text annotations)
- Extensions — Centerlines / Threading — circle center cross-lines, thread representations
