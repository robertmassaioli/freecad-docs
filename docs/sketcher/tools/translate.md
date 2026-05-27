# Move / Array Transform

> **In one sentence:** Move selected geometry to a new position, or create a
> rectangular grid of copies at specified row and column spacing.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher tools → Move / Array Transform  
**Toolbar:** Sketcher Tools · **Shortcut:** `W`

The **Translate** tool (`Sketcher_Translate`) moves geometry or creates a linear
array. When a single copy is created (count = 1 × 1) it is a pure move; when
more rows or columns are specified it produces a rectangular grid of copies.

!!! note "Legacy tool"
    FreeCAD also has an older `Sketcher_RectangularArray` command. The newer
    `Sketcher_Translate` ("Move / Array Transform") is the recommended replacement
    for FreeCAD 1.0+ and supports both move and array in one tool.

---

## Intuition

Think of a rubber stamp: you press it once to get the original mark, then slide
the stamp and press again for a copy. Move / Array Transform lets you slide once
(move) or lay out a grid of stamps in rows and columns (array). The geometry is
duplicated, not linked — each copy is independent.

---

## When to use it

- You drew geometry in the wrong position and need to move it to its correct
  location.
- You need a linear row of identical features (bolt hole pattern, fin array,
  teeth on a rack).
- You need a rectangular grid pattern (PCB mounting holes, bolt circle grid).

## When NOT to use it

- **You need a polar (circular) array** — use [Rotate / Polar Transform](rotate.md).
- **You need copies linked by constraints** — use [Clone](copy-clone-move.md);
  clones share constraints with the original.
- **You need a mirror** — use [Mirror](symmetry.md).

---

## Step-by-step walkthrough

1. **Select the geometry** to move or copy (++ctrl++-click multiple elements,
   or use a box selection).

2. **Press `W`** or activate **Move / Array Transform**.

3. The task panel shows:
    - **Number of copies (i, j):** rows and columns. Set to 1,1 for a pure move.
    - **Translation vector:** the X and Y offset for each step.

4. **Click a base point** in the viewport (the reference for the translation).

5. **Click the destination** to set the translation vector, or type the X and Y
   offsets numerically in the task panel.

6. **Set the number of rows (i) and columns (j):** use 1 for each for a simple
   move; use higher values for an array.

7. **Click OK.** The geometry (and copies if count > 1) appear at the new
   position(s).

!!! tip
    For a row array, set columns j=1 and increase rows i. For a column array,
    do the reverse. For a 2-D grid, set both i and j greater than 1.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Copies (i) | Integer ≥ 1 | 1 | Number of copies in the primary direction. 1 = move only (no extra copies). |
| Copies (j) | Integer ≥ 1 | 1 | Number of copies in the secondary direction. |
| Translation vector | 2-D vector (X, Y) | Interactive | Offset for each copy step in the primary direction. |
| Secondary vector | 2-D vector (X, Y) | Interactive | Offset in the secondary direction (for 2-D arrays). |
| Clone | Toggle | Off | If on, copies are Clones (constraints linked to original). If off, copies are independent. |

---

## Common mistakes and pitfalls

!!! warning "Geometry moves but constraints are not updated"
    **Cause:** Move / Array Transform repositions geometry but does not add
    distance constraints to the new positions. The moved elements may be
    under-constrained at the new location.  
    **Fix:** After moving, add [Lock Position](constraint-lock.md) or
    [Dimension](dimension.md) constraints to fix the new positions.

!!! warning "Array copies overlap the original"
    **Cause:** The translation vector is (0, 0) or too small.  
    **Fix:** Set a translation vector that equals the feature spacing.

---

## Python API

```python
import FreeCAD as App
import Sketcher

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Move/array is invoked via GUI; the underlying method for rectangular array:
# sketch.addRectangularArray(geoIds, translation_vector, constrain, columns, rows, clone, step)
# TODO: verify signature against SketchObject source.
print("Use the Move/Array Transform tool interactively.")
```

---

## See also

- [Rotate / Polar Transform](rotate.md) — polar (circular) array
- [Copy / Clone / Move](copy-clone-move.md) — non-array copy and move operations
- [Symmetry (Mirror)](symmetry.md) — mirror-based copy
- [Lock Position](constraint-lock.md) — constrain moved geometry position
