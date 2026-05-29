# Dimension

> **In one sentence:** Measures a distance, radius, or angle and displays the
> result with a parametric dimension line that updates when the geometry changes.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Annotation → Dimension  
**Command:** `Draft_Dimension`

Dimensions are **parametric** — they update automatically if you snapped to
the solid geometry when creating them. Fixed-coordinate dimensions do not update.

---

## Dimension types

| Type | How to create |
|------|--------------|
| Linear distance | Pick two points, then pick the dimension line position |
| Radius | Pick a circle or arc edge, then pick the label position |
| Diameter | Same as radius — toggle with the Diameter button |
| Angular | Pick two edges or three points to define the angle |

---

## Step-by-step (linear)

1. Activate **Draft → Annotation → Dimension**.
2. Pick the **start point** of the measurement.
3. Pick the **end point**.
4. Pick the **dimension line position** (how far off the measured geometry).
5. The dimension is placed with the measured value as text.

---

## Key properties

| Property | Description |
|----------|-------------|
| Start | First measurement point |
| End | Second measurement point |
| Dimline | Position of the dimension line |
| Override | Optional text to override the measured value |
| Decimal places | Number of decimal digits shown |
| Show unit | Whether to append the unit symbol |
| Flip text | Flip the text above/below the dimension line |

---

## Python API

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

## Common mistakes and pitfalls

!!! warning "Dimensions don't update after geometry changes"
    If a dimension was placed by clicking on free 3-D space (not snapping
    to an edge or vertex of a solid), it stores a fixed coordinate and will
    not update. Always snap to the actual edge or vertex for linked dimensions.

---

## See also

- [Text](text.md) — free-form text annotation
- [Label](label.md) — leader line with callout
- [Annotation Style Editor](annotation-style-editor.md) — consistent arrow and font settings
- [Draft Workbench](../index.md) — workbench overview
