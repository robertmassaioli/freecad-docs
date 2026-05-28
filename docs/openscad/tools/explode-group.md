# Explode Group

> **In one sentence:** Break apart a Boolean fusion or compound into its
> individual component objects, each shown as a separate visible object.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Explode Group  
**Shortcut:** none

Dissolves a `Part::Fuse`, `Part::MultiFuse`, or `Part::Compound` into its
constituent parts. The container object is deleted and each sub-object is
revealed as an independent, individually visible object. Random colours are
assigned to components that had the default colour, making them visually
distinguishable.

---

## Intuition

A boolean fusion or compound holds multiple objects bundled together. Explode
Group is the inverse: it un-bundles them, returning each object to its
independent existence so you can work with them separately.

---

## When to use it

- You want to inspect or modify individual components of a compound after
  import (many STEP files import as compounds).
- You accidentally fused objects and want to separate them without losing
  either shape.
- You want to apply operations to each sub-object independently.

---

## When NOT to use it

- **Deeply nested compounds** — Explode Group only unpacks one level at a
  time. Nested compounds need to be exploded multiple times.
- **On objects with children that have parents** — Explode Group only works
  on top-level objects (no parents in the document tree).

---

## Step-by-step

1. Select a `Part::Fuse`, `Part::MultiFuse`, or `Part::Compound`.
2. Choose **OpenSCAD → Explode Group**.
3. The compound is deleted and each sub-object appears individually.
4. Components that had default colour are assigned a random colour.

---

## Parameters

No parameters — operates on the selected compound.

---

## Common mistakes and pitfalls

!!! warning "Explode Group does nothing on non-compound objects"
    **Cause:** The selected object is not a Fuse, MultiFuse, or Compound.  
    **Fix:** Use this tool only on compound-type objects. For other groupings,
    use document tree operations.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_ExplodeGroup', 0)")
```

---

## See also

- [Expand Placements](expand-placements.md) — apply placements before exploding
- [OpenSCAD Workbench](../index.md) — workbench overview
