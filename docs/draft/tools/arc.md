# Arc

> **In one sentence:** Draws a circular arc by specifying centre point,
> radius, start angle, and aperture angle.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Arc Tools → Arc  
**Command:** `Draft_Arc`

Classic three-step arc construction: centre → radius → start angle → end angle.
Use [Arc by 3 Points](arc-3points.md) if you know points the arc must pass through.

---

## Step-by-step

1. Activate **Draft → Drafting → Arc Tools → Arc**.
2. Pick the **centre point**.
3. Move the cursor to set the **radius** (or type a value).
4. Click to set the **start angle** on the circle.
5. Click to set the **end angle** (the aperture).

---

## Key properties

| Property | Description |
|----------|-------------|
| Radius | Circle radius |
| First Angle | Start angle in degrees, measured from X-axis |
| Last Angle | End angle — arc sweeps from First to Last |

---

## Python API

```python
import Draft

arc = Draft.make_circle(
    radius=30,
    face=False,
    startangle=0,
    endangle=90,
    placement=App.Placement(App.Vector(0, 0, 0), App.Rotation())
)
App.ActiveDocument.recompute()
```

---

## See also

- [Arc by 3 Points](arc-3points.md) — arc through three known points
- [Circle](circle.md) — full circle
- [Draft Workbench](../index.md) — workbench overview
