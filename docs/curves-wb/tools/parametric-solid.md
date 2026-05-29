# Parametric Solid

> **In one sentence:** Creates a closed solid from a set of selected surfaces
> (shells) — the parametric equivalent of Part's "Convert to Solid".

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Misc. → Parametric Solid  
**Command:** `solid`

The selected faces must form a closed, watertight shell. The result is a
`Part::Feature` solid that updates if the source surfaces change — unlike
Part's Convert to Solid which produces a static result.

---

## See also

- [Blend Surface](blend-surface.md) — create the connecting surfaces to close the shell
- [Extract Shapes](extract-shapes.md) — isolate sub-shapes to work with before joining
- [Curves Workbench](../index.md) — workbench overview
