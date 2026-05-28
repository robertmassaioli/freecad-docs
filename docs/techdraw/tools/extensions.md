# Extension Pack

> **In one sentence:** The Extension Pack adds professional drawing-finishing
> tools — format and align dimensions, draw centerlines and thread
> representations, add prefix characters, and control cosmetic circle
> geometry — all in a single organised package.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Attributes/Modifications / Centerlines/Threading / Format/Organize Dimensions

The Extension Pack is divided into three groups:

| Group | Menu | Purpose |
|-------|------|---------|
| [Attributes and Modifications](#attributes-and-modifications) | Attributes/Modifications | Line style, extend/shorten, lock views, cascade dimensions |
| [Centerlines and Threading](#centerlines-and-threading) | Centerlines/Threading | Circle centerlines, bolt circles, thread symbols, cosmetic arcs/circles |
| [Format and Organize Dimensions](#format-and-organize-dimensions) | Format/Organize Dimensions | Chain dims, coordinate dims, chamfer dims, prefix/suffix, decimal places |

---

## Intuition

The core TechDraw tools (Insert View, Insert Dimension, etc.) create the
basic content of a drawing. The Extension Pack refines that content to meet
professional drawing standards:

- **Alignment** — engineering drawings require dimensions to be aligned in
  chains or cascades so they are easy to read and comply with ISO/ASME
  drawing standards.
- **Thread representations** — ISO/ASME standard thread representation uses
  specific line patterns (solid for crest, dashed for root in hidden views,
  etc.) that the Extension Pack draws automatically.
- **Prefix characters** — diameter (⌀), square section (□), and repetition
  (n×) symbols are required by drawing standards; the Extension Pack inserts
  them without manual text editing.
- **Decimal consistency** — all dimensions on a drawing must have the same
  number of decimal places; Increase/Decrease Decimal adjusts them in bulk.

---

## Attributes and Modifications

### Select Line Attributes

**Menu:** TechDraw → Attributes/Modifications → Select Line Attributes

Opens a palette for choosing the **style**, **width**, and **colour** that
will be applied to new cosmetic lines. This is a "set tool state" operation —
it does not change existing lines, only new ones created afterwards.

The saved attributes are used by:
- Add Cosmetic Line Through 2 Points
- Draw Cosmetic Circle / Arc (in Centerlines/Threading)
- Add Parallel / Perpendicular Line (in Centerlines/Threading)

---

### Change Line Attributes

**Menu:** TechDraw → Attributes/Modifications → Change Line Attributes

Applies the currently saved line attributes (from Select Line Attributes) to
all **selected** cosmetic lines. Select the lines first, then run this command.

Use this to bulk-restyle cosmetic lines — change colour, dash pattern, or
weight across a selection in one operation.

---

### Extend Line / Shorten Line

**Menu:** TechDraw → Attributes/Modifications → Extend Line / Shorten Line

Lengthens or shortens a selected cosmetic line by a fixed amount at one or
both ends. Enter the delta length in the task panel.

Common use: extend a centerline so it protrudes beyond the part boundary by
the standard amount required by your drawing standard (ISO requires
2–5 mm overhang for centerlines).

---

### Lock / Unlock View

**Menu:** TechDraw → Attributes/Modifications → Lock / Unlock View

Prevents a view from being accidentally moved on the page when locked.
Locking is indicated by a padlock icon on the view frame.

Lock views once the layout is finalised to prevent accidental displacement
while adding dimensions and annotations.

---

### Position Section View

**Menu:** TechDraw → Attributes/Modifications → Position Section View

Forces a section view to align exactly horizontally or vertically with its
parent view, snapping it to the correct projection relationship. Use this
after manually moving a section view that has drifted out of alignment.

---

### Position Horizontal / Vertical / Oblique Chain

**Menu:** TechDraw → Attributes/Modifications → Position Horizontal/Vertical/Oblique Chain Dimension

Aligns a set of selected dimensions so their dimension lines are exactly
co-linear — forming a horizontal chain, vertical chain, or oblique chain.

Select all the dimensions in the chain first, then run the command. The
dimensions snap to a common baseline.

---

### Cascade Horizontal / Vertical / Oblique Dimensions

**Menu:** TechDraw → Attributes/Modifications → Cascade Horizontal/Vertical/Oblique Dimension

Distributes selected dimensions at **equal spacing** along the
horizontal/vertical/oblique axis. Cascading produces the stacked parallel
dimension layout commonly seen in mechanical drawings.

**Workflow:**

1. Add all the dimensions first.
2. Select them all (click first, Ctrl-click remaining).
3. Run Cascade Horizontal Dimensions.
4. Set the spacing value in the task panel.
5. Click **OK**. The dimensions redistribute evenly.

---

### Calculate Area Annotation / Arc Length Annotation

**Menu:** TechDraw → Attributes/Modifications → Calculate Area Annotation / Calculate Arc Length Annotation

Reads the area of a selected face or the arc length of a selected edge and
places the computed value as a text annotation on the view. Updates
automatically if the geometry changes.

---

### Customise Dimension Format

**Menu:** TechDraw → Attributes/Modifications → Customise Dimension Format

Opens a dialog to set the format string (`FormatSpec`) for selected
dimensions. The format string uses printf-style notation:

| Format | Output |
|--------|--------|
| `%.0f` | No decimal places (integer) |
| `%.1f` | One decimal place |
| `%.2f` | Two decimal places (default) |
| `%.3f` | Three decimal places |
| `%g` | Significant figures (removes trailing zeros) |

You can add prefix and suffix text alongside the format code:

| FormatSpec | Output example |
|------------|----------------|
| `"⌀%.2f"` | ⌀25.00 |
| `"R%.1f"` | R12.5 |
| `"%.2f ±0.05"` | 25.00 ±0.05 |

---

## Centerlines and Threading

### Add Circle Centerlines

**Menu:** TechDraw → Centerlines/Threading → Add Circle Centerlines

Draws a pair of crossing centerlines (a + cross) through the centre of
selected circles or arcs. The lines are drawn in center-line style
(long-dash-short-dash) and extend beyond the circle by the standard
overhang amount.

**Step-by-step:**

1. Click one or more circles in a view (or select all at once with a
   window selection).
2. Choose **TechDraw → Centerlines/Threading → Add Circle Centerlines**.
3. Centerlines appear on all selected circles.

---

### Add Bolt Circle Centerlines

**Menu:** TechDraw → Centerlines/Threading → Add Bolt Circle Centerlines

Draws the pitch circle diameter (PCD) and radial centerlines for a bolt-hole
pattern. Select all the bolt holes in the pattern at once; the tool computes
the common circle centre and draws the PCD arc plus one radial line per hole.

---

### Thread Representations

The Extension Pack provides four thread representation tools conforming to
ISO 6410 / ASME Y14.6 simplified thread conventions:

| Tool | Menu name | What it draws |
|------|-----------|---------------|
| Thread Hole (Side) | Draw Thread Hole (Side View) | Inner thread: solid minor diameter, dashed major diameter |
| Thread Hole (Bottom) | Draw Thread Hole (Bottom View) | Inner thread: full minor circle, ¾ major circle |
| Thread Bolt (Side) | Draw Thread Bolt (Side View) | External thread: solid major diameter, dashed minor diameter |
| Thread Bolt (Bottom) | Draw Thread Bolt (Bottom View) | External thread: full major circle, ¾ minor circle |

**Step-by-step for all thread tools:**

1. Click the circular edge of the hole or bolt in the view.
2. Choose the appropriate thread representation tool.
3. The cosmetic lines appear in the correct ISO style.
4. Use Insert Diameter Dimension to dimension the thread nominal diameter,
   then manually set the `FormatSpec` to add the thread designation
   (e.g. `"M12 × 1.75"`).

---

### Add Vertex at Intersection

**Menu:** TechDraw → Centerlines/Threading → Add Vertex at Intersection

Adds a cosmetic vertex at the geometric intersection of two selected edges.
Use when you need to dimension to a corner that has been filleted (the
sharp corner no longer exists as a vertex, but the intersection of the
extended edge lines does).

---

### Add Offset Vertex

**Menu:** TechDraw → Centerlines/Threading → Add Offset Vertex

Adds a cosmetic vertex offset from an existing vertex by a specified X, Y
distance. Use for dimensioning to a theoretical point that is a known offset
from a physical corner.

---

### Cosmetic Circle / Arc Tools

| Tool | Description |
|------|-------------|
| Add Cosmetic Circle | Draw a circle by centre point and radius |
| Draw Cosmetic Circle (2 Points) | Draw a circle through centre and rim point |
| Draw Cosmetic Circle (3 Points) | Draw a circle through three points |
| Draw Cosmetic Arc | Draw an arc by centre, start, and end points |

These tools draw circles and arcs that exist only on the drawing (not in
the 3-D model). Common uses:
- Pitch circle diameter on a flange with bolt holes.
- Theoretical engagement circle for a gear.
- Construction arcs for locating features relative to a known radius.

---

### Draw Parallel / Perpendicular Line

| Tool | Description |
|------|-------------|
| Draw Parallel Line | Draw a cosmetic line parallel to a selected edge |
| Draw Perpendicular Line | Draw a cosmetic line perpendicular to a selected edge |

Click the reference edge, then click the point through which the new line
should pass. The new cosmetic line is drawn in the current saved line style.

---

## Format and Organize Dimensions

### Chain Dimensions

| Tool | Description |
|------|-------------|
| Create Horizontal Chain Dimension | Dimension a series of features end-to-end horizontally |
| Create Vertical Chain Dimension | Dimension a series of features end-to-end vertically |
| Create Oblique Chain Dimension | Dimension along an oblique direction |

A **chain dimension** (also called a running dimension) places consecutive
dimensions so the end of one is the start of the next, forming an
unbroken chain across multiple features.

**Step-by-step (Horizontal Chain):**

1. Select three or more vertices in a view (the start point + one vertex
   per feature gap).
2. Choose **TechDraw → Format/Organize Dimensions → Create Horizontal Chain Dimension**.
3. Set the spacing (distance from the part to the first dimension line).
4. Click **OK**. One dimension is created per interval.

---

### Coordinate Dimensions

| Tool | Description |
|------|-------------|
| Create Horizontal Coordinate Dimension | Baseline ordinate dimensions — all measured from one datum |
| Create Vertical Coordinate Dimension | Vertical baseline ordinate |
| Create Oblique Coordinate Dimension | Oblique baseline ordinate |

**Coordinate dimensions** (ordinate dimensioning) measure all features from
a single datum baseline. All dimension lines run from the datum to each
feature; there is no accumulation of tolerance errors as in chain dimensions.
Preferred for CNC machined parts.

**Step-by-step:**

1. Select the datum vertex first, then all other vertices in order.
2. Choose **Create Horizontal Coordinate Dimension**.
3. The datum is automatically assigned zero; all other dimensions measure
   from it.

---

### Chamfer Dimensions

| Tool | Description |
|------|-------------|
| Create Horizontal Chamfer Dimension | Dimension a chamfer with the horizontal distance |
| Create Vertical Chamfer Dimension | Dimension a chamfer with the vertical distance |

Click the two endpoints of a chamfer line. The tool creates a dimension
showing the chamfer size in the standard `C × 45°` or `size × angle` format.

---

### Create Arc Length Dimension

**Menu:** TechDraw → Format/Organize Dimensions → Create Arc Length Dimension

Dimensions the arc length of a selected curved edge (the distance along
the curve, not the chord length). The arc-length symbol (~) is added
automatically.

---

### Prefix Tools

| Tool | Shortcut effect | Description |
|------|-----------------|-------------|
| Insert Diameter Prefix | Prepends ⌀ | Add the diameter symbol before the dimension value |
| Insert Square Prefix | Prepends □ | Add the square cross-section symbol |
| Insert Repetition Prefix | Prepends n× | Add a count multiplier (e.g. 4× for four identical holes) |
| Remove Prefix Character | Removes first char | Strip any single-character prefix |

**Step-by-step for all prefix tools:**

1. Select one or more dimensions on the page.
2. Choose the desired prefix tool.
3. The prefix is prepended to (or removed from) the dimension text immediately.

These tools edit the `FormatSpec` property of the dimension.

---

### Increase / Decrease Decimal Places

| Tool | Description |
|------|-------------|
| Increase Decimal Places | Add one decimal place to all selected dimensions |
| Decrease Decimal Places | Remove one decimal place from all selected dimensions |

Select multiple dimensions at once and apply in one step to ensure
consistent decimal precision across the drawing. The tools modify the
`FormatSpec` printf precision specifier.

---

## Common mistakes and pitfalls

!!! warning "Select Line Attributes must be set before drawing new lines"
    Select Line Attributes saves a tool state. If you draw a cosmetic line
    without setting attributes first, it uses whatever state was last saved
    (or the default). Set attributes before drawing cosmetic geometry if
    you need a specific style.

!!! warning "Chain dimensions accumulate tolerance"
    Chain dimensioning (end-to-end) accumulates manufacturing tolerance at
    each step. For precision features, use coordinate (baseline) dimensions
    instead. This is a fundamental GD&T principle, not a FreeCAD limitation.

!!! warning "Thread tools draw cosmetic lines — not actual threads"
    The thread representation tools draw ISO-conventional cosmetic lines.
    They do not create any thread geometry in the 3-D model and do not
    affect hole or shaft dimensions. You still need the correct nominal
    dimension and thread designation text.

!!! warning "Prefix tools modify FormatSpec — check after applying"
    Prefix tools prepend text to the `FormatSpec` field. If you apply
    multiple prefixes in sequence, both are prepended (e.g. `⌀R%.2f`).
    Check the dimension text after applying prefixes.

---

## See also

- [Dimensions](dimensions.md) — the base dimension tools the Extension Pack formats
- [Annotations and Hatching](annotations.md) — centerlines placed via the core tools
- [Symbols](symbols.md) — thread-related ISO symbols (hole/shaft fit)
