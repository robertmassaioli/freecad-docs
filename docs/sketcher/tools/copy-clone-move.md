# Copy / Clone / Move

> **In one sentence:** Duplicate selected sketch geometry as an independent copy
> (Copy), as a constraint-linked clone (Clone), or move it to a new position
> (Move) — without creating an array.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher tools → Copy / Clone / Move  
**Toolbar:** Sketcher Tools (CompCopy dropdown) · **Shortcut:** none

| Tool | Description |
|------|-------------|
| Copy | Create an independent duplicate of selected geometry (constraints are not linked) |
| Clone | Create a duplicate whose geometry is linked to the original via Equal constraints |
| Move | Reposition selected geometry to a new location (no copy made) |

These are the non-array single-copy/move tools. For grids or polar arrays, use
[Move / Array Transform](translate.md) or [Rotate / Polar Transform](rotate.md).

!!! note
    FreeCAD also exposes **Copy Elements**, **Cut Elements**, and **Paste Elements**
    clipboard commands (`Ctrl+C`, `Ctrl+X`, `Ctrl+V`) for copying geometry
    between sketches via the system clipboard.

---

## Intuition

**Copy** is "duplicate and forget": the new geometry starts at the same position as
the original but has no ongoing relationship with it. Change the original, and the
copy is unaffected.

**Clone** is "duplicate and remember": FreeCAD automatically adds Equal constraints
between corresponding edges and Coincident-relative constraints so the clone always
has the same shape as the original. Move the original, and the clone stays put but
keeps the same shape.

**Move** has no copy involved — it picks up the geometry and places it elsewhere.

---

## When to use them

- **Copy:** You need an identical shape at a different position in the sketch but
  want to constrain each independently (e.g. two holes with different positions but
  same size — use Equal for size, position each separately).
- **Clone:** You need multiple features that must always have the same shape (a row
  of identical connector tabs). Changing the shape of one automatically updates all
  clones.
- **Move:** You drew geometry in the wrong place and want to relocate it. (Also
  see [Move / Array Transform](translate.md) for move with array support.)

## When NOT to use them

- **You need a full array (multiple copies at regular spacing)** — use
  [Move / Array Transform](translate.md) or [Rotate / Polar Transform](rotate.md).
- **You need the copy to be parametrically driven from a spreadsheet** — use
  a formula-driven constraint instead; Copy/Clone provides only geometric linking.

---

## Step-by-step walkthrough

### Copy

1. **Select geometry** to copy.
2. **Activate Copy** from the CompCopy dropdown or menu.
3. **Click a base point** (the reference point for the copy offset).
4. **Click the destination** (where the base point should land in the copy).
5. The copy appears. Its geometry is independent; add constraints as needed.

### Clone

1. **Select geometry** to clone.
2. **Activate Clone** from the dropdown.
3. **Click base and destination** as with Copy.
4. The clone appears. Equal constraints are automatically added between original
   and clone edges. Changing the original's shape updates the clone.

### Move

1. **Select geometry** to move.
2. **Activate Move** from the dropdown.
3. **Click base and destination** to set the translation.
4. The geometry relocates. Existing constraints may need adjustment.

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| Base point | 2-D coordinate | The reference point on the original geometry |
| Destination point | 2-D coordinate | Where the base point moves to (copy/clone) or final position (move) |

---

## Common mistakes and pitfalls

!!! warning "Clone constraints make the sketch over-constrained"
    **Cause:** Clone adds Equal constraints for all corresponding edges; if those
    edges already have independent dimensional constraints, conflicts arise.  
    **Fix:** Before cloning, remove individual dimensional constraints from the
    elements you plan to clone. Set dimensions on the original only; the clone
    will match via Equal.

!!! warning "Move leaves geometry under-constrained"
    **Cause:** Move relocates geometry but does not add constraints to the new
    position.  
    **Fix:** After moving, apply [Lock Position](constraint-lock.md) or
    [Coincident](constraint-coincident.md) to fix the moved geometry at its new
    location.

---

## Python API

```python
import FreeCAD as App

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Copy geometry element 0 with offset (20, 10, 0)
# The result is new geometry at the offset position.
# TODO: verify exact method signature for copy in SketchObject
print("Use the Copy/Clone/Move tools interactively for now.")
```

---

## See also

- [Move / Array Transform](translate.md) — rectangular arrays
- [Rotate / Polar Transform](rotate.md) — polar arrays
- [Symmetry (Mirror)](symmetry.md) — reflected copies
- [Equal](constraint-equal.md) — manually link sizes between copies (what Clone does automatically)
