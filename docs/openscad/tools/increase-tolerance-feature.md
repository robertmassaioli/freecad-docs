# Increase Tolerance Feature

> **In one sentence:** Wrap a shape with a feature that increases its OCCT
> geometric tolerance to help boolean operations that fail on near-coincident
> geometry.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Increase Tolerance Feature  
**Shortcut:** none

Creates a `Part::FeaturePython` wrapper around the selected shape that increases
the OpenCASCADE geometric tolerance from the default 1 × 10⁻⁷ mm to a higher
value. The source object is hidden. Used as a last-resort repair technique when
boolean operations fail because vertices or edges are just slightly outside the
tolerance threshold.

---

## Intuition

FreeCAD's geometry kernel (OCCT) uses a tolerance threshold to decide whether
two points, edges, or vertices are "the same". If two vertices are 2 × 10⁻⁷ mm
apart, OCCT considers them different; boolean operations then fail at that
junction. Increase Tolerance Feature tells OCCT to treat a slightly larger
distance as "the same", allowing the operation to succeed.

---

## When to use it

- A Boolean Union, Intersection, or Difference fails with "bad result" or
  "invalid shape" on geometry that visually looks correct.
- You suspect near-coincident vertices or edges are causing the failure.
- Other repair methods (Refine Shape Feature, ShapeHeal) have not helped.

---

## When NOT to use it

- **As a first resort** — always try fixing the geometry root cause first.
  Increased tolerance makes the shape less precise.
- **On high-precision parts** — the tolerance increase degrades geometric
  accuracy. Document the deviation from nominal when using this on precision
  parts.

---

## Step-by-step

1. Select the `Part::Feature` that is causing the boolean to fail.
2. Choose **OpenSCAD → Increase Tolerance Feature**.
3. A `Part::FeaturePython` is created; the original is hidden.
4. Retry the boolean operation on the new tolerance-increased feature.

---

## Parameters

No task panel — the tolerance increase is a fixed default step.

---

## Common mistakes and pitfalls

!!! warning "Increased tolerance makes the shape less precise"
    **Cause:** By design — this is a repair workaround, not a clean solution.  
    **Fix:** After the boolean succeeds, consider re-modelling the original
    geometry to avoid the near-coincident topology.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_IncreaseToleranceFeature', 0)")
```

---

## See also

- [Refine Shape Feature](refine-shape-feature.md) — remove seam edges
- [OpenSCAD Workbench](../index.md) — workbench overview
