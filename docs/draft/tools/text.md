# Text

> **In one sentence:** Places a multi-line text annotation at a picked point
> in 3-D model space.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Annotation → Text  
**Command:** `Draft_Text`

Display-only — no solid geometry. For formal engineering drawing annotations,
use TechDraw instead.

---

## Step-by-step

1. Activate **Draft → Annotation → Text**.
2. Pick the insertion point in the 3-D view.
3. Type the text in the input panel. Press **Enter** to add lines.
4. Click **Create text** (or press **Enter** on the last line) to finish.

---

## Key properties

| Property | Description |
|----------|-------------|
| Text | List of text lines |
| Text Size | Character height in model units |
| Font Name | Font family name |
| Justification | Left / Right / Centre |
| Placement | Position and orientation in 3-D space |

---

## Python API

```python
import Draft

text = Draft.make_text(
    ["First line", "Second line"],
    placement=App.Placement(App.Vector(0, 0, 0), App.Rotation())
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Text faces the camera only in Screen mode"
    Draft Text is placed at a fixed orientation in 3-D space. If you
    rotate the view, the text may appear sideways or backwards. Set
    the **Display Mode** property to `Screen` to always face the camera.

---

## See also

- [Dimension](dimension.md) — dimension line with measured value
- [Label](label.md) — leader line with callout text
- [Annotation Style Editor](annotation-style-editor.md) — consistent text formatting
- [Draft Workbench](../index.md) — workbench overview
