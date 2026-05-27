# Merge Sketches

> **In one sentence:** Combine two or more separate sketches into a single new
> sketch, collecting all geometry and constraints from each source.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Merge Sketches  
**Toolbar:** Sketcher · **Shortcut:** none

Merge Sketches takes multiple selected sketch objects and produces one new sketch
that contains the union of all their geometry and constraints. The source sketches
are not deleted; the merged result is a new, independent object.

---

## Intuition

Think of photocopying two separate drawings onto one sheet of paper: both drawings
are now on the same surface and can interact with each other (share constraints,
form a single closed profile), but the original sheets still exist unchanged.

This is useful when you have built a complex sketch in two halves in separate
objects — perhaps because you mirrored one half — and now need them in a single
sketch to serve as a closed profile for a Pad or Pocket.

---

## When to use it

- You used [Mirror Sketch](mirror-sketch.md) to generate the opposite half of a
  symmetric profile and now need both halves as one closed wire.
- You received multiple sketch objects that together form one complete profile and
  you want to consolidate them.
- You are migrating a design that was split across sketches for organisational
  reasons into a single parameter set.

## When NOT to use it

- **You want to keep sketches logically separate** — merging is not easily
  reversible; if you want to keep the originals separate, work with them as inputs
  to a Loft or ShapeBinder instead.
- **You want to re-use one sketch's geometry in another without merging** — use
  [Carbon Copy](external-geometry.md#carbon-copy) to import a copy.

---

## Step-by-step walkthrough

1. **Select two or more sketches** in the Model tree. Hold ++ctrl++ to select
   multiple objects.

2. **Activate Merge Sketches** via **Sketch → Merge Sketches**.

3. A new sketch object appears in the Model tree containing the geometry and
   constraints from all selected sketches.

4. **Open the merged sketch** to verify the geometry. If the source sketches were
   drawn on different planes, you may need to add constraints to connect the
   imported geometry.

!!! tip
    If the merged sketch is over-constrained after merging (source sketches each had
    constraints that are now redundant with each other), delete the duplicate
    constraints in the merged sketch's edit mode.

---

## Parameters

Merge Sketches has no parameter dialog. The result is a new sketch object with an
auto-generated name.

---

## Common mistakes and pitfalls

!!! warning "Merged sketch is open (not a closed profile)"
    **Cause:** The source sketches were separate open wires. After merging, the
    combined geometry is still open.  
    **Fix:** Open the merged sketch, add coincident constraints or closing lines to
    join the loose endpoints, and close the profile.

!!! warning "Merged sketch has too many DoF"
    **Cause:** Each source sketch was individually constrained, but after merging the
    constraints that fixed their mutual positions (e.g. which side of the origin each
    was on) are now missing.  
    **Fix:** Add constraints in the merged sketch to position the formerly separate
    geometry relative to each other.

---

## Python API

```python
# TODO: verify — Merge Sketches does not appear to expose a direct
# Python API call. It is invoked via the GUI command only.
# The resulting object is a standard Sketcher::SketchObject.
```

---

## See also

- [Mirror Sketch](mirror-sketch.md) — create a mirrored copy of a sketch
- [Carbon Copy](external-geometry.md#carbon-copy) — copy geometry from one sketch into another
- [New Sketch / Edit Sketch](new-sketch.md) — create and manage sketches
