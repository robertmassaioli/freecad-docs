# To Console

> **In one sentence:** Prints the selected shape's properties to FreeCAD's
> Python console as a script fragment — for programmatic inspection or
> reproducing geometry in code.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Misc. → To Console  
**Command:** `to_console`

Output includes:

- Shape type and topology summary
- Key geometric properties
- For edges: the underlying curve object with its parameters

---

## When to use it

- Inspecting or reproducing a shape's geometry programmatically.
- Diagnosing unexpected shape behaviour by examining raw geometric data.
- Extracting data to use in a FreeCAD macro or external script.

---

## See also

- [B-Spline to Console](bspline-to-console.md) — detailed B-spline knot/pole export
- [Geometry Info](geometry-info.md) — interactive geometric properties panel
- [Curves Workbench](../index.md) — workbench overview
