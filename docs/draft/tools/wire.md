# Wire (Polyline)

> **In one sentence:** Draws a connected sequence of straight line segments
> by clicking successive points; can be closed into a face.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Drafting → Wire  
**Command:** `Draft_Wire`

Creates a `Draft::Wire` with two or more vertices. Each click adds a segment.
The wire can remain open or be closed back to the first point to form a polygon.

---

## Step-by-step

1. Activate **Draft → Drafting → Wire**.
2. Click points one by one to add segments.
3. Press **A** to toggle **Relative mode** (offset from previous point).
4. Press **C** to close the wire back to the first point.
5. Press **Enter** or double-click the last point to finish open.

---

## Key properties

| Property | Description |
|----------|-------------|
| Points | List of all vertices |
| Closed | Whether the last vertex connects to the first |
| Make Face | Create a filled face when Closed is True |
| Fillet Radius | Optional radius to fillet all corners |
| Chamfer Size | Optional chamfer on all corners |

---

## Python API

```python
import Draft

pts = [
    App.Vector(0, 0, 0),
    App.Vector(50, 0, 0),
    App.Vector(50, 50, 0),
    App.Vector(0, 50, 0),
]
wire = Draft.make_wire(pts, closed=True, face=True)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Make Face requires a coplanar closed wire"
    If Make Face is True but the wire is not closed or its vertices are not
    coplanar, FreeCAD cannot compute a face. Check the Closed property and
    verify all points share the same Z value.

---

## See also

- [Line](line.md) — single two-point segment
- [Fillet](fillet.md) — add fillet arc between two lines
- [Snapping](snap-lock.md) — precise vertex placement
- [Draft Workbench](../index.md) — workbench overview
