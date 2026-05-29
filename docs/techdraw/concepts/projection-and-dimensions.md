# Projection Methods and Dimension Types

> **In one sentence:** Understanding first-angle vs third-angle projection
> determines where views appear on a multi-view drawing; understanding
> projected vs 3-D referenced dimensions determines whether a measurement
> is geometrically correct or only visually approximate.

---

## Orthographic projection

An **orthographic view** is a 2-D projection of a 3-D object — every line
of sight is parallel to the projection direction, so there is no perspective
foreshortening. Each face of the object is seen at true shape when the
projection direction is perpendicular to that face.

A standard engineering drawing uses **multiple orthographic views** —
typically front, top, and right — to fully describe a 3-D object. The views
are arranged on the page according to a projection convention.

---

## First-angle vs third-angle projection

The two conventions differ in which *quadrant* the object is projected into.
The practical effect is **where each view lands on the page**.

### First-angle projection (ISO / European)

The object is between the viewer and the projection plane. The projected
image lands on the **opposite side** from the viewer.

```
Viewer  →  Object  →  Projection plane
```

**View arrangement:**

```
                  [Left view]
[Top view]  [Front view]  [Right view]  [Back view]
                  [Bottom view]
```

In first angle: the **top view is below the front view**; the **right view
is to the left** of the front view. This is the opposite of what intuition
might suggest.

### Third-angle projection (ANSI / American)

The projection plane is between the viewer and the object. The image lands
on the **same side** as the viewer.

```
Viewer  →  Projection plane  →  Object
```

**View arrangement:**

```
                  [Top view]
[Left view]  [Front view]  [Right view]  [Back view]
                  [Bottom view]
```

In third angle: the **top view is above the front view**; the **right view
is to the right** of the front view — matching natural intuition.

---

## Comparison table

| | First angle (ISO) | Third angle (ANSI) |
|--|------------------|--------------------|
| Standard | ISO 128-30 | ANSI Y14.3 |
| Common in | Europe, Asia, international | USA, Canada, Mexico |
| Top view position | Below front | Above front |
| Right view position | Left of front | Right of front |
| Projection symbol | ●→○ (filled cone toward circle) | ○→● (circle toward filled cone) |

The **projection symbol** (a truncated cone drawn in the title block) tells
the reader which convention is used. Always include it on drawings that
will be read internationally.

---

## Setting the projection in TechDraw

Set the projection type in one of two places:

**Per Projection Group:**
In the Projection Group's **Data** panel, change **Projection Type** to
`First Angle` or `Third Angle`.

**Document default:**
**Edit → Preferences → TechDraw → General → Projection Angle**

When you change the Projection Type on an existing group, all dependent
views automatically reposition to the new arrangement.

---

## Recommended view sets

Most engineering drawings use a subset of the six possible views. Choose
the minimum number that unambiguously describes the part:

| Part type | Typical view set |
|-----------|-----------------|
| Prismatic / block | Front + Top + Right (3 views) |
| Symmetric / turned part | Front + Right only (2 views) |
| Flat plate | Top + section (2 views) |
| Complex casting | Front + Top + Right + 1–2 sections |
| Sheet metal | Front + unfolded flat pattern |

Add an **isometric view** for assembly instructions or 3-D context — but
note that isometric views are not suitable for dimensioning.

---

## Projected vs 3-D referenced dimensions

This is the most important dimension concept in TechDraw, and the most
common source of incorrect drawing measurements.

### Projected dimensions (2-D)

When you dimension by selecting **edges in the drawing view** (2-D geometry),
TechDraw measures the **projected length on the drawing plane** — the
foreshortened distance as seen in the view, not the true 3-D distance.

For edges that are **parallel to the drawing plane**, projected length equals
true length — the measurement is correct.
For edges that are **angled toward or away from the viewer**, projected length
is **shorter than the true 3-D length** — the measurement is wrong.

### 3-D referenced dimensions (true distance)

When you dimension by selecting **3-D vertices** (via **Shift-click** to
access the 3-D geometry), TechDraw measures the **true 3-D distance** between
those vertices — regardless of the view angle or foreshortening.

This is the correct approach for all dimensions that must be manufacturable.

### How to tell which type you have

- **Projected:** the dimension shows the 2-D distance on the drawing plane.
  It can change if you rotate the view.
- **3-D referenced:** the dimension shows the true 3-D distance. It does not
  change if you rotate the view. A small **`(∅)`** or **`☞`** annotation
  may appear on the dimension line.

### Practical rules

| Situation | Use |
|-----------|-----|
| Edge is parallel to the view plane | Either type — they give the same result |
| Edge is perpendicular to the view plane (foreshortened) | 3-D referenced only |
| Diagonal edge at angle to the view plane | 3-D referenced only |
| Circle / arc in correct view | Projected diameter/radius — fine |
| Axonometric / isometric view | Use [Axonometric Length Dimension](../tools/dim-axonometric.md) |

!!! warning "Projected dimensions on angled edges are misleading"
    A wall at 45° that appears 70.7 mm long in a top view is actually 100 mm
    long. Never use projected dimensions for edges that are not parallel to
    the drawing plane — the dimension will be wrong.

!!! tip "Use Insert Dimension (smart) for correct type selection"
    The [Insert Dimension](../tools/dim-insert.md) tool analyses the selected
    geometry and picks the right type automatically. For explicit control,
    hold Shift to access 3-D vertices when placing a dimension.

---

## Broken views and partial views

For long or repetitive parts, use:

- **[Broken View](../tools/view-broken.md)** — removes a section of the
  middle of a long part; the break lines indicate that the part continues.
- **[Detail View](../tools/view-detail.md)** — magnified inset of a small
  region at a larger scale.
- **[Section View](../tools/view-section.md)** — cross-section cut at a
  specified plane to reveal internal geometry.

---

## Python API

```python
import FreeCAD as App
import TechDraw

doc = App.ActiveDocument
page = doc.getObject("Page")
body = doc.getObject("Body")

# Create a projection group (third-angle)
group = doc.addObject("TechDraw::DrawProjGroup", "Views")
page.addView(group)
group.Source = [body]
group.ProjectionType = "Third Angle"   # or "First Angle"
group.Scale = 1.0

# Add front, top, and right views
group.addProjection("Front")
group.addProjection("Top")
group.addProjection("Right")
group.Anchor.Direction = App.Vector(0, -1, 0)   # Front looks along -Y
doc.recompute()

# Add a 3-D referenced length dimension (true distance)
dim = doc.addObject("TechDraw::DrawViewDimension", "TrueLength")
dim.Type = "Distance"
# References2D references 3-D vertices via "Vertex0" etc. (true 3-D)
view = doc.getObject("Views_Front")
dim.References2D = [(view, "Vertex0"), (view, "Vertex3")]
page.addView(dim)
doc.recompute()
```

---

## See also

- [Insert Projection Group](../tools/view-projection-group.md) — create a multi-view set
- [Insert View](../tools/view-insert.md) — single orthographic view
- [Insert Dimension](../tools/dim-insert.md) — smart dimension tool
- [Insert Length Dimension](../tools/dim-length.md) — explicit straight-line distance
- [Insert Axonometric Length Dimension](../tools/dim-axonometric.md) — true length on isometric view
- [Insert Section View](../tools/view-section.md) — cross-section cut
- [Insert Detail View](../tools/view-detail.md) — magnified inset
- [TechDraw Workbench](../index.md) — workbench overview
