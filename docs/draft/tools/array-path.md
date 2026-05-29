# Path Array

> **In one sentence:** Distributes copies of an object at evenly-spaced
> positions along a curve, optionally aligning each copy to the path tangent.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Array Tools → Path Array  
**Command:** `Draft_PathArray`

---

## Parameters

| Property | Description |
|----------|-------------|
| Path Object | The wire, edge, or spline to distribute along |
| Count | Number of copies |
| Align | Rotate each copy to follow the path tangent |
| Align Mode | Original / Frenet / Tangent |
| Tan Vector | Local axis to align with the path tangent |
| Start Offset / End Offset | Fraction of path length to leave empty at start/end |

---

## Align modes

| Mode | Description |
|------|-------------|
| Original | Keep the original orientation — no alignment |
| Frenet | Align with the natural Frenet-Serret frame of the path |
| Tangent | Align a chosen local axis with the path tangent |

---

## Python API

```python
import Draft

post = App.ActiveDocument.getObject("Body")
path = App.ActiveDocument.getObject("Wire")
arr = Draft.make_path_array(
    obj=post,
    pathobjwire=path,
    count=10,
    use_link=True,
    align=True
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Alignment requires a smooth path"
    If the path has sharp corners (a polyline), the tangent changes
    discontinuously at each vertex. Copies near corners may flip or rotate
    unexpectedly. Use a smooth B-spline path for well-behaved aligned arrays.

---

## See also

- [Array Path Link](array-path-link.md) — link-instance variant for large arrays
- [Array Path Twisted](array-path-twisted.md) — path array with twist rotation
- [Array Ortho](array-ortho.md) — rectangular grid
- [Draft Workbench](../index.md) — workbench overview
