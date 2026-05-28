# Views

> **In one sentence:** Views are live 2-D projections of 3-D geometry onto
> the drawing page — from a simple front-face orthographic to section cuts,
> magnified details, multi-view projection groups, and captures from other
> workbenches.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Views / Views From Other Workbenches

| Tool | Description |
|------|-------------|
| [Insert View](#insert-view) | Standard orthographic projection |
| [Insert Broken View](#insert-broken-view) | Long part with middle removed |
| [Insert Section View](#insert-section-view) | Cross-section cut through a view |
| [Insert Complex Section](#insert-complex-section) | Offset or aligned section |
| [Insert Detail View](#insert-detail-view) | Magnified inset |
| [Insert Projection Group](#insert-projection-group) | First- or third-angle multiview set |
| [Insert Clip Group](#insert-clip-group) | Rectangular clip frame |
| [Insert SVG Symbol](#insert-svg-symbol) | SVG file as a static symbol |
| [Insert Bitmap Image](#insert-bitmap-image) | Raster image on the page |
| [Share View](#share-view) | Repeat a view on multiple pages |
| [Toggle Frames](#toggle-frames) | Show / hide view frames |
| [Project Shape](#project-shape) | Project edges without a view container |
| [Insert Active View](#insert-active-view) | Snapshot of the 3-D viewport |
| [Insert Draft View](#insert-draft-view) | Draft workbench objects |
| [Insert Arch View](#insert-arch-view) | Arch / BIM workbench sections |
| [Insert Spreadsheet View](#insert-spreadsheet-view) | Spreadsheet as a drawing table |

---

## Intuition

Every view in TechDraw is a **live link** to 3-D geometry. When you change
the 3-D model — resize a pad, move a hole, add a fillet — all views that
project that object update automatically on recompute. You never manually
redraw geometry.

The important architectural point is that TechDraw never stores a copy of
the geometry — it re-projects from the current model state every time.
This means:

- **Parametric drawings are the default.** A drawing made in TechDraw is
  always consistent with the current model.
- **Deleted or renamed geometry breaks the view.** If you delete a body
  that a view references, the view turns red and stops updating.
- **View direction is fixed at creation time.** The view stores a direction
  vector; you can change it in the view's properties but not by rotating
  the 3-D model.

---

## Insert View

**Menu:** TechDraw → Views → Insert View

Creates a standard orthographic projection of a selected 3-D object onto
the drawing page. This is the most fundamental TechDraw operation.

### What can be projected

- A **Part Design Body**
- A **Part Shape** (any object with a Shape property)
- An **Assembly** (projects the full assembled state)
- A **Part::Feature** (any solid, shell, or compound)

### Step-by-step

1. Select the 3-D object(s) in the model tree.
2. Choose **TechDraw → Views → Insert View** (or click the toolbar button).
3. The view appears on the active page at the default direction (looking
   down the -Y axis by default — a front view in FreeCAD's coordinate
   convention).
4. In the model tree, the view appears as a child of the page.
5. Click the view on the page to select it. Drag it to reposition.
6. In the **Data** panel, change **Direction** to set the projection
   direction. Common values:

| View | Direction |
|------|-----------|
| Front | `(0, -1, 0)` |
| Top | `(0, 0, 1)` |
| Right | `(1, 0, 0)` |
| Isometric | `(1, -1, 1)` (normalised) |

### Scale

The view **Scale** property sets the drawing scale relative to the model.
`1.0` = 1:1 (model mm = drawing mm). `0.5` = 1:2 (half size). `2.0` = 2:1
(double size).

### View properties (Data panel)

| Property | Description |
|----------|-------------|
| Direction | Projection direction vector |
| Scale | Drawing scale ratio |
| Scale Type | Custom / Page / Automatic |
| Rotation | Rotation of the view on the page (degrees) |
| CoarseView | Faster rendering at lower quality (useful for large assemblies) |
| ShowHidden | Show hidden edges as dashed lines |
| ShowSmoothLines | Show smooth (tangent) edge transitions |
| ShowSeamLines | Show seam lines on surfaces |

### Python API

```python
import FreeCAD as App
import TechDraw

doc = App.ActiveDocument
page = doc.getObject("Page")
body = doc.getObject("Body")

view = doc.addObject("TechDraw::DrawViewPart", "FrontView")
page.addView(view)
view.Source = [body]
view.Direction = App.Vector(0, -1, 0)   # Front view
view.Scale = 1.0
doc.recompute()
```

---

## Insert Broken View

**Menu:** TechDraw → Views → Insert Broken View

Creates a view of a long object (a shaft, beam, or plate) with the middle
section visually removed, bringing the two ends closer together on the page.
The dimension values still reflect the full un-broken length.

### When to use it

- Long slender parts (shafts, bars, extrusions) where the majority of the
  length is uniform and does not need to be shown in full.
- Fitting a long part onto a standard paper size without reducing scale so
  much that detail is lost.

### Step-by-step

1. Insert a standard **Insert View** of the object first.
2. Select that view on the page.
3. Choose **TechDraw → Views → Insert Broken View**.
4. In the task panel, drag the break lines to the desired positions.
5. Choose the break line style (zigzag, straight, or S-curve).
6. Click **OK**.

The break indicators appear on the view, and the rendered gap is removed.
Dimensions on the broken view still display the full model length.

---

## Insert Section View

**Menu:** TechDraw → Views → Insert Section View

Creates a cross-section view by cutting an existing view with a plane. The
section shows the internal geometry exposed by the cut, with hatching on
the cut faces.

### Step-by-step

1. Select an existing view on the page (the parent view that will be cut).
2. Choose **TechDraw → Views → Insert Section View**.
3. In the task panel:
   - Set the **Cut Direction** (normal to the cut plane).
   - Position the cut plane by dragging the line on the parent view, or
     enter an exact offset value.
4. Click **OK**. The section view appears on the page, labelled
   automatically (A–A, B–B, etc.).

### Properties

| Property | Description |
|----------|-------------|
| BaseView | The parent view that is cut |
| Normal | Normal vector of the cutting plane |
| Origin | A point on the cutting plane |
| Scale | Independent scale for the section view |
| ShowCutSurface | Show hatching on cut faces |
| CutSurfaceDisplay | Hatching pattern or colour |

### Python API

```python
section = doc.addObject("TechDraw::DrawViewSection", "SectionA")
page.addView(section)
section.BaseView = front_view
section.SectionNormal = App.Vector(0, 1, 0)    # cut plane faces Y
section.SectionOrigin = App.Vector(0, 0, 0)
doc.recompute()
```

---

## Insert Complex Section

**Menu:** TechDraw → Views → Insert Complex Section

Creates an offset section or aligned section — a section cut that follows a
bent or stepped path rather than a single flat plane. Used when a single
straight cut would miss important features.

### Section types

- **Offset Section** — the cut plane is stepped: it starts at one plane,
  jumps to a parallel plane, then continues. Each step is perpendicular to
  the view direction.
- **Aligned Section** — the cut plane is hinged: it consists of multiple
  planes that meet at a common axis, like a broken wing. Each segment is
  rotated into the projection plane for display.

### Step-by-step

1. Select the parent view.
2. Choose **TechDraw → Views → Insert Complex Section**.
3. In the task panel, select the section type (Offset or Aligned).
4. Draw the cut path by clicking points on the parent view.
5. Click **OK**.

---

## Insert Detail View

**Menu:** TechDraw → Views → Insert Detail View

Creates a magnified inset view of a circular region of an existing view.
A circle is drawn on the parent view to indicate the magnified area, and
the detail view appears on the page with its own label and scale.

### When to use it

- Small features (threads, fillets, close tolerances) that are too small
  to dimension clearly at the main view scale.
- Calling out a complex region without increasing the scale of the whole view.

### Step-by-step

1. Select the parent view on the page.
2. Choose **TechDraw → Views → Insert Detail View**.
3. In the task panel:
   - Click on the parent view to set the centre of the detail circle.
   - Set the circle **Radius** (in page mm, not model mm).
   - Set the **Scale** for the magnified view.
4. Click **OK**.

The detail view appears, labelled with a letter (C, D, …). A corresponding
circle with the same letter appears on the parent view.

---

## Insert Projection Group

**Menu:** TechDraw → Views → Insert Projection Group

Creates a coordinated set of orthographic views (front, top, right, and
optionally left, bottom, back, isometric) that are automatically aligned
on the page. This is the standard way to create a multi-view drawing.

### First-angle vs third-angle projection

The Projection Group supports both ISO first-angle and ANSI third-angle
projection conventions. Set the convention in the Projection Group's
**Projection Type** property or in
**Edit → Preferences → TechDraw → General → Projection Angle**.

| Convention | Standard | Symbol |
|------------|----------|--------|
| First angle | ISO / European | Circle with left cone |
| Third angle | ANSI / US | Circle with right cone |

### Step-by-step

1. Select the 3-D object in the model tree.
2. Choose **TechDraw → Views → Insert Projection Group**.
3. In the task panel:
   - Choose which views to include (Front, Top, Right, Left, Bottom, Back,
     Isometric).
   - Set the scale and anchor view direction.
4. Click **OK**. The views appear aligned on the page.

Moving the anchor view (the front view) moves all other views automatically,
maintaining their alignment.

---

## Insert Clip Group

**Menu:** TechDraw → Views → Insert Clip Group

Creates a rectangular clipping frame on the page. Views placed inside the
clip group are cropped to the frame's bounds. Useful when a view is large
but only a portion needs to be visible.

---

## Insert SVG Symbol

**Menu:** TechDraw → Views → Insert SVG Symbol

Inserts an SVG file as a static, non-parametric symbol on the drawing page.
Common uses: company logo, north arrow, general-note block, or any vector
artwork that does not need to update with the model.

The SVG is embedded in the document — you can move and scale it on the page.

---

## Insert Bitmap Image

**Menu:** TechDraw → Views → Insert Bitmap Image

Inserts a raster image (PNG, JPG, BMP) on the drawing page. Use for
photographs, scanned signatures, or any content that cannot be represented
as vector geometry.

---

## Share View

**Menu:** TechDraw → Views → Share View

Makes an existing view appear on a second page without duplicating it. Both
pages show the same live view object — changes to the view (direction,
scale, properties) apply everywhere it appears. Useful for title sheets that
show a key view alongside the detail pages.

---

## Toggle Frames

**Menu:** TechDraw → Views → Toggle Frames

Shows or hides the blue rectangular frames that surround each view on the
page. Frames are a UI aid — they show view boundaries and allow selection.
They do not appear in exported SVG or DXF files.

Toggle Frames off to see the clean drawing output as it will be exported.

---

## Project Shape

**Menu:** TechDraw → Views → Project Shape

Projects 3-D edges, wires, and vertices directly onto the drawing page
without creating a view container. The result is a set of 2-D SVG paths
that can be styled and moved freely. Unlike a normal view, a projected
shape does not update when the model changes.

Use for one-off reference geometry, custom diagram elements, or decorative
lines that are not meant to be parametric.

---

## Insert Active View

**Menu:** TechDraw → Views from Other Workbenches → Insert Active View

Captures the current state of the 3-D viewport — including any visual
styles (shaded, wireframe), render mode, and camera angle — as a static
raster image on the drawing page. This is a snapshot, not a parametric
projection.

### When to use it

- Adding a shaded isometric "beauty shot" to a drawing alongside the
  precise orthographic views.
- Capturing a specific rendering style or perspective that TechDraw's
  vector projection cannot reproduce.

---

## Insert Draft View

**Menu:** TechDraw → Views from Other Workbenches → Insert Draft View

Projects Draft workbench objects (wires, shapes, text, dimensions) onto a
TechDraw page. Draft objects are 2-D by nature; the Draft View renders them
at a specified scale.

---

## Insert Arch View

**Menu:** TechDraw → Views from Other Workbenches → Insert Arch View

Projects BIM / Arch workbench objects (walls, floors, spaces, sections)
onto a TechDraw page. The Arch View tool is designed for architectural plan
and section drawings, respecting Arch Section Plane objects.

---

## Insert Spreadsheet View

**Menu:** TechDraw → Views from Other Workbenches → Insert Spreadsheet View

Embeds a Spreadsheet workbench table on the drawing page. The table renders
as a grid of cells exactly as it appears in the Spreadsheet editor.

### Common use cases

- Parts list / bill of materials table placed on the same sheet as a
  projection group.
- Revision history table in the drawing.
- Parameter table showing design variables.

The spreadsheet view updates automatically when the spreadsheet content
changes.

---

## Common mistakes and pitfalls

!!! warning "Changing model body after inserting a view"
    If you rename or delete a body that a view references, the view displays
    a red warning and stops updating. Re-select the source object in the
    view's **Source** property.

!!! warning "Section view requires a parent view"
    Section View and Detail View must be created from an existing parent
    view. You cannot apply a section cut to the 3-D model directly —
    create a standard view first.

!!! warning "Insert View direction vs model orientation"
    TechDraw's default front view looks along –Y. FreeCAD's Part Design
    convention places the XY plane as the sketch plane — so the default
    front view of a Part Design body often shows the bottom face. Adjust
    the **Direction** property or set a custom direction in the Insert View
    task panel.

!!! warning "Projection Group anchor view must be the front view"
    The front view in a Projection Group is the anchor; all other views
    align relative to it. If you change the front view's direction, the
    other views rotate to match. Changing only a side view's direction
    breaks the alignment.

---

## See also

- [Pages](pages.md) — create the drawing page before inserting views
- Dimensions — add measurements to views
- Annotations — add text, leaders, and balloons
