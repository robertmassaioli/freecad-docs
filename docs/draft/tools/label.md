# Label

> **In one sentence:** Places a leader line with an arrowhead pointing to a
> 3-D object or point, plus a callout text box at the other end.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Annotation → Label  
**Command:** `Draft_Label`

The Draft equivalent of a balloon or callout annotation. Can display a
custom string or automatically show object properties (name, length, area,
volume, material, etc.).

---

## Step-by-step

1. Activate **Draft → Annotation → Label**.
2. Pick the **target point** — where the arrowhead points to.
3. Pick the **leader elbow point** — where the leader line bends.
4. Pick the **label position** — where the text box is placed.
5. In the task panel, select the **Label type** and enter any custom text.

---

## Label types

| Label type | What is displayed |
|------------|------------------|
| Custom | Text you type manually |
| Name | The object's name (model tree label) |
| Position | XYZ position of the target point |
| Length | Length of the selected edge |
| Area | Area of the selected face |
| Volume | Volume of the selected solid |
| Material | The material name assigned to the object |
| Tag | The object's Tag property (BIM/IFC data) |

---

## Key properties

| Property | Description |
|----------|-------------|
| Label Type | See table above |
| Custom Text | Text shown when Label Type = Custom |
| Target Point | Where the arrowhead points |
| Straight Distance | Length of the leader tail |
| Arrow Type | Dot, Arrow, Tick, etc. |

---

## Python API

```python
import Draft

label = Draft.make_label(
    targetpoint=App.Vector(50, 50, 0),
    direction="horizontal",
    distance=App.Vector(-30, 0, 0),
    labeltype="Custom",
    placement=App.Placement(App.Vector(0, 80, 0), App.Rotation())
)
label.CustomText = ["My label text"]
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Property labels require target on an object"
    Label types like Length, Area, Volume, and Material only work when the
    target point is on an actual object. If the arrow points at empty space,
    the label shows nothing.

---

## See also

- [Text](text.md) — free-form text without leader line
- [Dimension](dimension.md) — measured dimension with dimension line
- [Annotation Style Editor](annotation-style-editor.md) — arrow and font settings
- [Draft Workbench](../index.md) — workbench overview
