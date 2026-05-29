# Line

> **In one sentence:** Draws a single straight line segment between two picked
> or typed points on the active working plane.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Line  
**Command:** `Draft_Line`

Creates a `Draft::Wire` with two points. Use [Wire](wire.md) instead when
you need more than one connected segment.

---

## Step-by-step

1. Activate **Draft → Drafting → Line**.
2. Pick or type the start point (X, Y, Z).
3. Pick or type the end point.
4. Press **Enter** or click **Close** to finish.

---

## Key properties

| Property | Description |
|----------|-------------|
| Start / End | Explicit coordinates of the endpoints |
| Line Color | RGB line colour |
| Line Width | Display line width |
| Subdivision Type | None / Linear subdivisions |

---

## Python API

```python
import Draft

line = Draft.make_line(
    App.Vector(0, 0, 0),   # start
    App.Vector(100, 0, 0)  # end
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Object placed at unexpected height"
    If the line appears at an unexpected elevation, the working plane is
    not what you expect. Check and reset with **Draft → Utilities →
    Select Working Plane**.

---

## See also

- [Wire](wire.md) — multi-segment polyline
- [Snapping](snap-lock.md) — precise placement
- [Select Working Plane](select-working-plane.md) — control which plane geometry is drawn on
- [Draft Workbench](../index.md) — workbench overview
