# Refine Shape Feature

> **In one sentence:** Wrap a solid in a feature that removes redundant seam
> edges left behind by boolean operations, leaving only topologically necessary
> edges on smooth faces.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Refine Shape Feature  
**Shortcut:** none

Creates a `Part::FeaturePython` named `refine_<original>` that hides the
source object and applies the OpenCASCADE shape refinement algorithm to remove
seam edges — redundant edges on smooth faces that boolean operations leave
behind. Equivalent to enabling **Refine** in Part Boolean settings, but
applied as a discrete, removable feature.

---

## Intuition

When two faces are fused together by a Boolean Union, their shared boundary
becomes an edge even if both faces were perfectly smooth across it. These
"seam edges" appear as faint lines on otherwise smooth faces — they are
topologically harmless but visually distracting and can interfere with
downstream operations (fillets, STEP export, surface analysis). Refine Shape
Feature removes them in one step.

---

## When to use it

- A Boolean Cut or Fuse leaves unwanted lines across what should be a smooth
  face.
- You are preparing a shape for STEP export and want clean topology.
- You want to apply refinement as an explicit, reversible step (rather than
  using the "Refine" checkbox in boolean settings).

---

## When NOT to use it

- **When you need the seam edges** — some seam edges mark the boundary
  between different materials or surface zones; removing them loses that
  semantic information.

---

## Step-by-step

1. Select one `Part::Feature` object.
2. Choose **OpenSCAD → Refine Shape Feature**.
3. The original object is hidden and a new `refine_<name>` object appears.
4. To remove the refinement, delete `refine_<name>` and unhide the original.

---

## Parameters

No task panel — operates automatically on the selected object.

---

## Common mistakes and pitfalls

!!! warning "Visible edges remain after refinement"
    **Cause:** Some edges that look like seams are real topological edges
    (e.g. where two flat faces meet at a sharp angle). Refine only removes
    truly redundant edges.  
    **Fix:** The remaining edges are real. Use Part → Fillet to soften them,
    or accept them as genuine topology.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_RefineShapeFeature', 0)")
```

---

## See also

- [OpenSCAD Workbench](../index.md) — workbench overview
