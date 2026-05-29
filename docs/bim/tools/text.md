# Text

> **In one sentence:** Place a 3-D text annotation at a point in the BIM
> model that scales with the working plane.

---

## Overview

**Workbench:** BIM  
**Menu:** Annotation → Text  
**Command:** `BIM_Text`

Places a 3-D text annotation at a picked point in the model. The text is a
`Draft::Text` object — it can be moved and rotated like any object, and it
scales with the working plane.

---

## When to use it

- Labelling rooms, spaces, and elements directly in the 3-D view.
- Adding notes or callouts to the model.
- Marking reference points during design.

## When NOT to use it

- **For drawing sheet annotations** — use [Drawing Page](drawing-page.md)
  and [Drawing View](drawing-view.md) with TechDraw's own annotation tools.
- **For dimension lines** — use [Dimension (Aligned)](dimension-aligned.md).

---

## Common mistakes and pitfalls

!!! warning "Text appears very small at building scale"
    FreeCAD's default text size is in mm and can be tiny at architectural
    scale. Set the `FontSize` property explicitly, or configure an annotation
    style with a building-scale font size.

---

## See also

- [Dimension (Aligned)](dimension-aligned.md) — measured dimension annotations
- [Leader](leader.md) — leader line with text callout
- [BIM Workbench](../index.md) — workbench overview
