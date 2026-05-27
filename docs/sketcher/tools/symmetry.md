# Mirror (Symmetry Transform)

> **In one sentence:** Create a mirrored copy of selected sketch geometry about
> a chosen line within the sketch, adding the mirror image as new geometry in the
> same sketch.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher tools → Mirror  
**Toolbar:** Sketcher Tools · **Shortcut:** none

The Symmetry transform tool (`Sketcher_Symmetry`) copies selected geometry and
reflects it about a selected line (or sketch axis) within the same sketch. Unlike
[Mirror Sketch](mirror-sketch.md), which produces a separate sketch object, this
tool adds the mirrored geometry *inside the current sketch* as additional edges.

---

## Intuition

Think of folding a piece of paper and pressing hard: the ink on one side transfers
to the other, giving you an exact mirror image on the same sheet. Symmetry transform
does this in the sketch — you draw one half, select it, pick the mirror line, and
the other half appears immediately in the same sketch.

---

## When to use it

- You are drawing a symmetric profile and want to draw only one half manually then
  mirror it to complete the sketch.
- You want to add a symmetric feature inside an existing sketch without creating a
  new sketch object.
- You need a reflected copy of a set of construction lines.

## When NOT to use it

- **You want to mirror a whole sketch as a new object** — use
  [Mirror Sketch](mirror-sketch.md).
- **You want the two halves to remain parametrically linked by a Symmetric
  constraint** — use the [Symmetric constraint](constraint-symmetric.md) on the
  key points instead; Symmetry transform creates independent copies.
- **You want a 3-D mirror of a solid feature** — use
  [Part Design Mirrored](../../part-design/tools/mirrored.md).

---

## Step-by-step walkthrough

1. **Select the geometry** to mirror (the geometry to be reflected, not the mirror
   line).
2. **++ctrl++-click the mirror line** (a line in the sketch, or the sketch's
   horizontal/vertical axis).
3. **Activate Mirror** via **Sketch → Sketcher tools → Mirror** or the toolbar
   button.
4. The mirrored copy appears on the other side of the line. Both the original and
   the mirror are in the sketch.

!!! tip
    To mirror about the sketch Y-axis, ++ctrl++-click the vertical axis line before
    activating Mirror. The sketch axes (horizontal and vertical centre lines through
    the origin) are always available as mirror references.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Geometry to mirror | Selection | The edges and points to be reflected. |
| Mirror axis | Line selection | The line about which to reflect. Can be any line in the sketch or the sketch axes. |
| Delete original | Toggle (Off) | If on, removes the source geometry after mirroring. |

---

## Common mistakes and pitfalls

!!! warning "Mirrored geometry has no constraints and adds extra DoF"
    **Cause:** Symmetry Transform copies geometry but does not copy constraints.
    Each mirrored element adds its DoF to the sketch.  
    **Fix:** Apply [Symmetric](constraint-symmetric.md) constraints to key points,
    or apply [Coincident](constraint-coincident.md) and [Equal](constraint-equal.md)
    constraints to ensure the mirror geometry tracks the original.

---

## Python API

```python
# Mirror/Symmetry is invoked via the GUI command.
# The underlying operation uses SketchObject symmetry helpers.
# TODO: verify whether a direct API call exists for the in-sketch mirror.
print("Use the Mirror tool interactively.")
```

---

## See also

- [Symmetric Constraint](constraint-symmetric.md) — live parametric symmetry
- [Mirror Sketch](mirror-sketch.md) — mirror a whole sketch as a new object
- [Move / Array Transform](translate.md) — translate geometry or make linear arrays
- [Rotate / Polar Transform](rotate.md) — polar copies
