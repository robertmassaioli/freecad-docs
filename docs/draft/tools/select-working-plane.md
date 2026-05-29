# Select Working Plane

> **In one sentence:** Sets the active working plane — the plane on which all
> Draft geometry is created.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Utilities → Select Working Plane  
**Command:** `Draft_SelectPlane`

---

## Presets

| Preset | Normal vector |
|--------|--------------|
| Top (XY) | +Z |
| Front (XZ) | +Y |
| Side (YZ) | +X |
| Custom | From selected face or 3 points |
| Working Plane Proxy | Restored from a saved proxy object |

---

## Setting from a face

1. Select a face of an existing solid.
2. Activate **Select Working Plane**.
3. The working plane snaps to that face.

---

## Setting from 3 points

Activate **Select Working Plane** with nothing selected, then click three
non-collinear points in the 3-D view to define the plane.

---

## Python API

```python
import WorkingPlane

wp = WorkingPlane.get_working_plane()
wp.align_to_point_and_axis(
    App.Vector(0, 0, 0),
    App.Vector(0, 0, 1),
    0
)
```

---

## Common mistakes and pitfalls

!!! warning "Working plane may reset after operations"
    Some operations (attaching to a face, switching workbenches) may reset
    the working plane to the default XY plane. Use a
    [Working Plane Proxy](working-plane-proxy.md) to reliably restore a
    non-standard plane.

---

## See also

- [Working Plane Proxy](working-plane-proxy.md) — save and restore a working plane
- [Toggle Grid](toggle-grid.md) — show the grid on the working plane
- [Draft Workbench](../index.md) — workbench overview
