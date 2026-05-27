# Validate Sketch

> **In one sentence:** Scan an existing sketch for missing coincident constraints,
> degenerate geometry, and solver errors, then let you fix them interactively.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Validate Sketch  
**Toolbar:** Sketcher · **Shortcut:** none

Validate Sketch opens a diagnostic task panel that analyses a sketch for common
problems: vertices that are geometrically coincident but lack a coincident
constraint, extremely short edges that cause solver instability, and constraints
that are flagged as degenerate. For each category of problem it offers a
one-click fix.

---

## Intuition

Think of Validate Sketch as a spell-checker for your sketch. It cannot re-draw
bad geometry, but it can spot the silent errors that cause a sketch to refuse
to close or produce corrupted solids downstream — specifically the case where
two line endpoints *look* joined but are actually two separate points sitting
on top of each other (a "false vertex gap").

This tool is most useful when you import geometry via DXF or IGES, or after
Carbon Copy / External Geometry operations, because those paths often produce
coincident-looking vertices that are not formally connected.

---

## When to use it

- A sketch imported from a DXF file reports that it is not closed or cannot be
  used as a profile, even though it looks perfectly closed in the viewport.
- After using [Carbon Copy](external-geometry.md#carbon-copy), some vertices appear
  to share a point but the sketch has more DoF than expected.
- You receive a sketch from someone else and want to audit it for constraint health.
- The solver shows conflicts or redundancies that you cannot find manually.

## When NOT to use it

- **Your sketch has genuine unconstrained degrees of freedom** — Validate Sketch
  will not add missing dimensional constraints; you must add those yourself.
- **You want to fix topologically broken geometry** (e.g. self-intersecting edges)
  — Validate Sketch cannot repair those; you must edit the geometry manually.

---

## Step-by-step walkthrough

1. **Close sketch edit mode** if you are currently inside the sketch (click Close
   in the Tasks panel or choose Sketch → Leave Sketch).

2. **Select the sketch** in the Model tree (single click).

3. **Activate Validate Sketch** via **Sketch → Validate Sketch**. The Validate
   Sketch task panel opens.

4. **Click "Analyse"** to scan the sketch. The panel reports:

    - **Missing coincident constraints** — pairs of vertices within a tolerance that
      are not formally constrained to be coincident.
    - **Degenerate constraints** — constraints that reference non-existent geometry
      (usually a leftover after deleting a geometry element).

5. **Review each category and click the corresponding "Fix" button** to automatically
   add the required constraints or delete the degenerate entries.

6. **Adjust the tolerance** (default 0.1 mm) if needed — larger tolerance catches
   more misaligned vertices; smaller tolerance ignores near-coincident vertices that
   are intentionally close but not connected.

7. **Click OK** to close the panel.

!!! tip
    After running Validate Sketch, check the solver status again. If the DoF count
    dropped toward zero, the missing constraints were the root cause of the
    under-constrained sketch.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Tolerance | Float (mm) | 0.1 | Maximum distance between two vertices for them to be considered "missing coincident". Increase if DXF geometry has small gaps. |

---

## Common mistakes and pitfalls

!!! warning "Validate Sketch does not enter edit mode"
    **Cause:** You must close sketch edit mode *before* running Validate Sketch — the
    tool analyses the sketch from outside.  
    **Fix:** Leave sketch edit mode (click Close / Leave Sketch), then run Validate
    Sketch.

!!! warning "False positives with intentional near-coincident vertices"
    **Cause:** A sketch that has two intentionally separate vertices very close
    together (e.g. a very short line) can be flagged as missing a coincident
    constraint.  
    **Fix:** Lower the tolerance before clicking Analyse so that only genuine gaps
    are detected. Alternatively, review each flagged pair before applying the fix.

---

## Python API

There is no direct Python API for running Validate Sketch programmatically. The
underlying constraint-adding logic uses `addConstraint`:

```python
import FreeCAD as App
import Sketcher

doc = App.activeDocument()
sketch = doc.getObject("Sketch")

# Manually add a coincident constraint between point 0 of geo 0
# and point 0 of geo 1 (equivalent to what Validate Sketch would add)
sketch.addConstraint(Sketcher.Constraint("Coincident", 0, 1, 1, 1))
doc.recompute()
```

---

## See also

- [New Sketch / Edit Sketch](new-sketch.md) — create and enter a sketch
- [External Geometry / Carbon Copy](external-geometry.md) — common source of un-constrained coincident vertices
- [Dimension](dimension.md) — add missing dimensional constraints
