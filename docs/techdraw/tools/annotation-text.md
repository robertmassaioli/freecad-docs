# Insert Annotation

> **In one sentence:** Places a plain multi-line text block on the drawing
> page — for general notes, tolerances, material callouts, or reference text.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Annotations → Insert Annotation

A text annotation is free-positioned and does not have a leader line. Use it
for notes that apply to the drawing generally, not to a specific feature.
For notes pointing at geometry, use [Insert Leader Line](annotation-leader.md).

---

## Step-by-step

1. Choose **TechDraw → Annotations → Insert Annotation**.
2. The annotation appears on the page. Double-click it to edit the text.
3. Drag it to position.

---

## Properties

| Property | Description |
|----------|-------------|
| Text | The annotation text (multi-line supported) |
| Font | Font family |
| TextSize | Text height in mm |
| TextColor | Text colour |
| MaxWidth | Width at which text wraps to the next line |

---

## Python API

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

## See also

- [Insert Rich Text Annotation](annotation-rich-text.md) — bold, italic, formatted text
- [Insert Leader Line](annotation-leader.md) — text with an arrowhead leader pointing at geometry
- [TechDraw Workbench](../index.md) — workbench overview
