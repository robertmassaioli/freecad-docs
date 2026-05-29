# Defeaturing

> **In one sentence:** Remove selected faces from a solid (and attempt to
> heal the resulting holes) to simplify the shape for FEA, CFD, or export.

---

## Overview

**Workbench:** Part · **Menu:** Part → Defeaturing  
**Toolbar:** Part Analysis · **Shortcut:** none

Defeaturing removes selected faces from a solid shape and attempts to extend
or patch the adjacent faces to close the resulting holes. The goal is to
simplify geometry — removing fillets, chamfers, small holes, logos, or other
fine features that are irrelevant for the intended downstream operation (such as
FEA meshing or CFD simulation).

---

## Intuition

A part from a CAD library often has hundreds of fillets, text engravings, and
fine features. Loading this directly into an FEA solver produces millions of
mesh elements for details that have negligible structural influence. Defeaturing
strips those features away, leaving a simplified shape that meshes efficiently.

It is the digital equivalent of filing down a fillet on a physical test specimen:
the core geometry remains, the fine feature is gone.

---

## When to use it

- Simplifying imported STEP parts for FEA or CFD analysis.
- Removing fillets and chamfers from a model to check the "sharp-edged" base
  geometry.
- Removing decorative features (text, logos, fine cosmetic features) that inflate
  file size and mesh complexity.

## When NOT to use it

- **Structural features** — never defeature loadbearing fillets or stress-
  concentration-reducing rounds before a structural analysis. They exist precisely
  because the stress field depends on them.
- **Features you plan to re-add parametrically** — defeature only for one-way
  simplification workflows (analysis, export). The defeatured shape is static.

---

## Step-by-step walkthrough

1. **Select the solid** in the model tree.
2. In the viewport, **Ctrl+click the faces** to remove (e.g., a fillet face, a
   small hole's cylindrical face).
3. **Part → Defeaturing.**
4. FreeCAD attempts to heal the surface after removing the selected faces.
5. Check the result with [Check Geometry](check-geometry.md).

---

## Common pitfalls

!!! warning "Defeaturing may fail on complex feature removal"
    OCCT's face removal algorithm cannot always heal the gap left by a removed
    face. If the adjacent faces cannot be extended to close the hole, Defeaturing
    reports a failure. Try selecting fewer faces or a simpler feature.

!!! warning "Result is a static shape"
    The defeatured shape is not parametric. Changes to the original solid do
    not propagate. Defeature as the last step before export or meshing, not as
    part of the live model.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a filleted box (has fillet faces to defeature)
box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 20; box.Height = 15
doc.recompute()

fillet = doc.addObject("Part::Fillet", "Filleted")
fillet.Base = box
fillet.Edges = [(i, 2.0, 2.0) for i in range(1, 13)]
doc.recompute()

# Defeaturing: remove a fillet face (use interactively to pick the face)
# Programmatic approach (OCCT RemoveFeature):
defeat = doc.addObject("Part::Defeaturing", "Simplified")
defeat.Source = fillet
# defeat.FacesToRemove = [fillet.Shape.Faces[12]]  # select fillet face by index
# doc.recompute()
print("Use Part → Defeaturing interactively to select faces to remove.")
```

---

## See also

- [Check Geometry](check-geometry.md) — verify the defeatured shape is valid
- [Refine Shape](refine-shape.md) — clean up residual edges after defeaturing
- [Simple Copy](simple-copy.md) — create a static snapshot before defeaturing
