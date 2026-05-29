# Ellipse

> **In one sentence:** Draws an ellipse or elliptical arc by picking two
> diagonal corners of its bounding rectangle.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Ellipse  
**Command:** `Draft_Ellipse`

The semi-major and semi-minor axes are derived from the bounding box you pick.
Set First Angle and Last Angle to draw an elliptical arc instead of a full ellipse.

---

## Key properties

| Property | Description |
|----------|-------------|
| MajorRadius | Semi-major axis length |
| MinorRadius | Semi-minor axis length |
| Make Face | Fill the ellipse with a face |
| First Angle / Last Angle | If set, draws an elliptical arc |

---

## Python API

```python
import Draft

ellipse = Draft.make_ellipse(
    majradius=50,
    minradius=25,
    face=True
)
App.ActiveDocument.recompute()
```

---

## See also

- [Circle](circle.md) — circular special case
- [Draft Workbench](../index.md) — workbench overview
