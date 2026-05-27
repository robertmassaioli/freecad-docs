# Mirror Sketch

> **In one sentence:** Create a new, independent sketch that is the mirror image
> of one or more selected sketches reflected about their horizontal or vertical
> axis.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Mirror Sketch  
**Toolbar:** Sketcher · **Shortcut:** none

Mirror Sketch takes one or more existing sketches and creates a new sketch
containing a mirrored copy of all their geometry. The mirror axis is the sketch's
own horizontal (X) or vertical (Y) axis. The result is a separate, standalone
sketch object — not a linked copy; it can be edited independently.

---

## Intuition

Think of folding a piece of tracing paper in half along the horizontal or vertical
axis: the drawing on one side appears mirrored on the other. Mirror Sketch does the
same: it reads the selected sketch, flips every line, arc, and circle across the
chosen axis, and writes the result into a new sketch object.

Because the output is independent, it is best used for one-off geometry
generation. If you need a mirrored *pattern that stays linked* to the original,
use [Symmetry (Mirror)](symmetry.md) inside the sketch instead.

---

## When to use it

- You have drawn one half of a symmetric part (e.g. the left half of a bracket)
  and want to generate the right half as a separate sketch to hand to Part Design.
- You need a flipped profile for a subtractive loft or pipe that must be the mirror
  of an existing profile.
- You want to produce a physically mirrored sketch to document or export.

## When NOT to use it

- **You want to mirror geometry *within* the current sketch** — use the
  [Symmetry (Mirror)](symmetry.md) transform tool inside sketch edit mode.
- **You want the mirrored copy to stay parametrically linked** — Mirror Sketch
  creates an independent copy; changing the original does not update the mirror.
- **You are working in Part Design and want a symmetric solid feature** — use
  [Mirrored](../../part-design/tools/mirrored.md) at the Part Design level instead.

---

## Step-by-step walkthrough

1. **Select one or more sketches** in the Model tree. Hold ++ctrl++ to select
   multiple sketches.

2. **Activate Mirror Sketch** via **Sketch → Mirror Sketch**.

3. A new sketch object appears in the Model tree containing the mirrored geometry.
   By default the mirror axis is the sketch's horizontal (X) axis.

4. **Inspect the result.** Enter the new sketch's edit mode to verify the geometry,
   then close it.

!!! tip
    To mirror about the vertical axis instead, open the mirrored sketch in edit
    mode, select all geometry, delete it, and re-run the mirror — there is no
    axis-selection dialog in FreeCAD 1.1. Alternatively, apply a Symmetric
    constraint inside the original sketch and extract the second half manually.

---

## Parameters

Mirror Sketch has no interactive parameter dialog. The axis used for mirroring
is fixed (horizontal axis of the source sketch). The output sketch name is
auto-generated.

---

## Common mistakes and pitfalls

!!! warning "Mirror of fully constrained sketch is under-constrained"
    **Cause:** Mirror Sketch copies geometry but *does not copy constraints*. The
    new sketch has the same shapes but none of the original constraints.  
    **Fix:** The mirrored sketch will typically be free-floating. If you need it
    constrained, add constraints manually in the mirrored sketch's edit mode, or
    use the [Lock Position](constraint-lock.md) constraint to anchor key points.

!!! warning "Constraints reference deleted geometry"
    **Cause:** If the original sketch had constraints referencing external geometry
    (via External Geometry edges), those references will not exist in the mirrored
    sketch.  
    **Fix:** Remove or replace those constraints in the mirrored sketch.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Sketcher

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Mirror Sketch is invoked via the GUI command in Python using:
import SketcherGui
# The result is a new sketch in the document after the command fires.
# TODO: verify — direct Python API for MirrorSketch may not be exposed.
```

### Key properties

The mirrored sketch is a standard `Sketcher::SketchObject` with no special
properties linking it to the original.

---

## See also

- [Symmetry (Mirror)](symmetry.md) — mirror geometry *inside* the current sketch
- [Merge Sketches](merge-sketches.md) — combine two sketches into one
- [New Sketch / Edit Sketch](new-sketch.md) — create and edit sketches
