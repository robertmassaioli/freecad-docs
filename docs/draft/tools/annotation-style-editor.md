# Annotation Style Editor

> **In one sentence:** Creates and manages named annotation style presets
> that control text height, font, arrow type, and colour across all Draft
> annotations.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Annotation → Annotation Style Editor  
**Command:** `Draft_AnnotationStyleEditor`

Named styles are applied to [Text](text.md), [Dimension](dimension.md), and
[Label](label.md) objects via their **Style** property. Changing the style
definition updates all objects using that style at once.

---

## Style properties

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
| Scale multiplier | All |

---

## Workflow

1. Open the Style Editor (**Draft → Annotation → Annotation Style Editor**).
2. Click **New** and give the style a name (e.g. `A4-standard`).
3. Set all properties for the style.
4. Click **OK**.
5. Select annotation objects in the model.
6. In the Properties panel, set the **Style** property to the named style.

---

## See also

- [Text](text.md) — text annotation
- [Dimension](dimension.md) — dimension line
- [Label](label.md) — leader-line callout
- [Draft Workbench](../index.md) — workbench overview
