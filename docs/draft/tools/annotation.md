# Annotation Tools

> **In one sentence:** The Annotation menu provides tools for adding text,
> linear/angular dimensions, and leader-line labels to Draft drawings, plus
> a style editor for managing consistent text and arrow formatting.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Annotation

| Tool | Description |
|------|-------------|
| [Text](#text) | Place a multi-line text annotation |
| [Dimension](#dimension) | Measure a distance, radius, or angle |
| [Label](#label) | Annotated leader line with callout text |
| [Annotation Style Editor](#annotation-style-editor) | Manage named annotation styles |

---

## Intuition

### Annotation vs TechDraw dimensions

Draft annotations live in the **3-D model space** and are always visible
in the 3-D view. TechDraw dimensions live in **drawing sheet space** and
are linked to 2-D views for formal engineering drawings.

Use Draft annotations for quick on-model measurements, design notes, and
labels during the modelling process. Use TechDraw dimensions for formal,
printable 2-D drawings.

### Annotation styles

The **Annotation Style Editor** manages named styles — presets for text
height, font, arrow type, line colour, and scale. Assign a style to any
Text, Dimension, or Label object to keep annotations consistent across the
document.

---

## Text

**Menu:** Draft → Annotation → Text  
**Command:** `Draft_Text`

Places a multi-line text annotation at a picked point in 3-D space. The
text is a display-only object — it has no solid geometry and does not
affect Part operations.

### Step-by-step

1. Activate **Draft → Annotation → Text**.
2. Pick the insertion point in the 3-D view.
3. Type the text in the input panel. Use the **Enter** key to add lines.
4. Click **Create text** (or press **Enter** on the last line) to finish.

### Key properties

| Property | Description |
|----------|-------------|
| Text | List of text lines (one string per line) |
| Text Size | Character height in model units |
| Font Name | Font family name |
| Justification | Left / Right / Centre |
| Placement | Position and orientation in 3-D space |

### Python API

```python
import Draft

text = Draft.make_text(
    ["First line", "Second line"],
    placement=App.Placement(
        App.Vector(0, 0, 0), App.Rotation()
    )
)
App.ActiveDocument.recompute()
```

---

## Dimension

**Menu:** Draft → Annotation → Dimension  
**Command:** `Draft_Dimension`

Measures a distance, radius, or angle and displays the result with a
dimension line. Dimensions are **parametric** — they update automatically
if the measured geometry changes.

### Dimension types

| Type | How to create |
|------|--------------|
| Linear distance | Pick two points, then pick the dimension line position |
| Radius | Pick a circle or arc edge, then pick the label position |
| Diameter | Same as radius — toggle with the **Diameter** button |
| Angular | Pick two edges or three points to define the angle |
| Radial (point-to-edge) | Pick a vertex and an edge |

### Step-by-step (linear)

1. Activate **Draft → Annotation → Dimension**.
2. Pick the **start point** of the measurement.
3. Pick the **end point**.
4. Pick the **dimension line position** (how far from the measured edge).
5. The dimension is placed with the measured value as text.

### Key properties

| Property | Description |
|----------|-------------|
| Start | First measurement point |
| End | Second measurement point |
| Dimline | Position of the dimension line |
| Override | Optional text to override the measured value |
| Unit override | Force a specific unit (mm, in, etc.) |
| Flip text | Flip the text above/below the dimension line |
| Decimal places | Number of decimal digits shown |
| Show unit | Whether to append the unit symbol |

### Linking to 3-D geometry

When you snap to a vertex or edge of a solid while creating a dimension,
Draft records the **topological reference** (face/edge/vertex). The
dimension then updates if the geometry changes — this is the parametric
link.

If you snap to a free 3-D point (not on any object), the dimension stores
a fixed coordinate and does **not** update with model changes.

### Python API

```python
import Draft

dim = Draft.make_linear_dimension(
    App.Vector(0, 0, 0),     # start
    App.Vector(100, 0, 0),   # end
    App.Vector(50, -20, 0)   # dimline position
)
App.ActiveDocument.recompute()
```

---

## Label

**Menu:** Draft → Annotation → Label  
**Command:** `Draft_Label`

Places a leader line with an arrowhead that points to a 3-D object or
point, plus a text box (callout) at the other end. Labels are the Draft
equivalent of a balloon or callout annotation.

### Step-by-step

1. Activate **Draft → Annotation → Label**.
2. Pick the **target point** — the location the arrowhead points to.
3. Pick the **leader elbow point** — where the leader line bends.
4. Pick the **label position** — where the text box is placed.
5. In the task panel, select the **Label type** and enter any custom text.

### Label types

| Label type | What is displayed |
|------------|------------------|
| Custom | Text you type manually |
| Name | The object's name (label in the model tree) |
| Label | The object's Label property |
| Position | XYZ position of the target point |
| Length | Length of the selected edge |
| Area | Area of the selected face |
| Volume | Volume of the selected solid |
| Tag | The object's Tag property (BIM/IFC data) |
| Material | The material name assigned to the object |
| Title | Document title |
| Exposure | Exposure property (BIM use) |

### Key properties

| Property | Description |
|----------|-------------|
| Label Type | See table above |
| Custom Text | Text shown when Label Type = Custom |
| Target Point | Where the arrowhead points |
| Straight Distance | Length of the leader tail (horizontal segment) |
| Straight Direction | Direction of the straight tail |
| Arrow Type | Dot, Arrow, Tick, etc. |

### Python API

```python
import Draft

label = Draft.make_label(
    targetpoint=App.Vector(50, 50, 0),
    direction="horizontal",
    distance=App.Vector(-30, 0, 0),
    labeltype="Custom",
    placement=App.Placement(
        App.Vector(0, 80, 0), App.Rotation()
    )
)
label.CustomText = ["My label text"]
App.ActiveDocument.recompute()
```

---

## Annotation Style Editor

**Menu:** Draft → Annotation → Annotation Style Editor  
**Command:** `Draft_AnnotationStyleEditor`

Opens a dialog to create, edit, and delete named **annotation styles** —
presets that control the visual appearance of all Draft annotations (Text,
Dimension, Label).

### Style properties

| Property | Applies to |
|----------|-----------|
| Font Name | Text, Dimension, Label |
| Font Size | Text, Dimension, Label |
| Font Color | Text, Dimension, Label |
| Line Width | Dimension, Label |
| Line Color | Dimension, Label |
| Arrow Type | Dimension, Label |
| Arrow Size | Dimension, Label |
| Show Unit | Dimension |
| Decimal Places | Dimension |
| Scale multiplier | All (adjusts size for viewport scale) |

### Workflow

1. Open the Style Editor.
2. Click **New** and give the style a name (e.g. `A4-standard`).
3. Set all properties for the style.
4. Click **OK**.
5. Select annotation objects in the model.
6. In the Properties panel, set the **Style** property to the named style.

All annotations using that style update immediately when you edit the style.

---

## Common mistakes and pitfalls

!!! warning "Dimensions don't update after geometry changes"
    If a dimension was placed by clicking on free 3-D space (not snapping
    to an edge or vertex of a solid), it stores a fixed coordinate. It will
    not update when the solid changes. Always snap to the actual edge/vertex
    when creating linked dimensions.

!!! warning "Label Target Point must reference an object"
    The Label type properties (Length, Area, Volume, Material, etc.) only
    work when the target point is on an actual object. If you set Label Type
    to `Area` but the arrow points at empty space, the label shows nothing.

!!! warning "Text is 3-D and must face the camera"
    Draft Text objects are placed at a fixed orientation in 3-D space. If
    you rotate the view, the text may appear sideways or backwards. The
    **Display Mode** property can be set to `Screen` to always face the
    camera.

---

## See also

- [Drafting Tools](drafting.md) — geometry creation
- [Annotation Style Editor](annotation.md#annotation-style-editor) — consistent formatting
- [Snapping](snapping.md) — precise placement for dimension points
