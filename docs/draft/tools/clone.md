# Clone

> **In one sentence:** Creates a parametric linked clone of the selected
> object that follows the original's shape changes and can be independently
> scaled and positioned.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Clone  
**Command:** `Draft_Clone`

---

## Clone vs Copy

| | Clone | Copy (Move with P) |
|--|-------|--------------------|
| Links to original | Yes — updates with original | No — independent |
| Independent scale | Yes — X/Y/Z independently | N/A |
| Can change shape | No — shape is always the original | Yes |

---

## Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
clone = Draft.make_clone([obj], delta=App.Vector(50, 0, 0))
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Scale applies in local coordinates"
    The Clone's X/Y/Z scale properties scale along the clone's local axes,
    not the global axes. If the clone is rotated, scaling X does not scale
    along the global X direction.

---

## See also

- [Move](move.md) — move/copy without parametric link
- [Array Ortho](array-ortho.md) — many clones in a grid
- [Draft Workbench](../index.md) — workbench overview
