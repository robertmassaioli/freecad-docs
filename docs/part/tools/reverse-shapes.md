# Reverse Shapes

> **In one sentence:** Create a copy of a shape with all face normals flipped,
> fixing inside-out solids and inverted mesh imports.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Reverse Shapes  
**Shortcut:** none

Creates a copy of the selected shape with all face normals reversed. The
geometry is identical — vertex positions and edge lengths are unchanged — but
the "outside" and "inside" of every face are swapped.

---

## Intuition

Face normals define which side of a face is the "outside". Correct normals
point outward from a solid's enclosed volume. Some imported shapes — particularly
from mesh conversion or certain STEP/IGES exporters — have normals pointing
inward, making the solid appear inside-out.

Symptoms of inverted normals:
- The shape renders black or very dark in the 3D view (normals point away from
  the camera)
- Boolean Cut or Fuse produces the inverse of what you expect
- Check Geometry reports reversed orientation

Reverse Shapes flips every normal without changing the geometry, correcting
these issues.

---

## When to use it

- A converted mesh shape (from [Shape from Mesh](shape-from-mesh.md)) renders
  with a dark/black appearance.
- A Boolean operation produces an empty result or the inverse of what is
  expected.
- Check Geometry reports "reversed" orientation on faces.
- A STEP or IGES import looks correct in shaded view but Boolean operations
  behave unexpectedly.

## When NOT to use it

- **Don't use it as a general-purpose copy tool** — use
  [Simple Copy](simple-copy.md) for that. Reverse Shapes is specifically for
  fixing normal orientation.
- **Don't use it on a correctly-oriented solid** — reversing normals on a
  correct solid will break it (it will then be inside-out).

---

## Step-by-step

1. Identify a shape with inverted normals (dark rendering, unexpected Boolean
   results, or Check Geometry error).
2. Select the shape in the model tree.
3. Choose **Part → Reverse Shapes**.
4. A new `Part::Feature` appears with all normals flipped.
5. Verify the rendering looks correct and re-run the Boolean operation.

---

## Parameters

This tool has no task panel. All normals are reversed automatically.

---

## Common mistakes and pitfalls

!!! warning "Reversing a correctly-oriented solid makes it inside-out"
    Only use Reverse Shapes on shapes that are confirmed to have inverted
    normals. Applying it to a correctly-oriented solid produces the same
    symptoms you were trying to fix.

!!! warning "The result is static"
    Reverse Shapes creates a non-parametric copy. If the original shape
    changes, the reversed copy is not updated.

!!! warning "Check normals before running Boolean operations"
    If a Boolean Cut produces nothing, check the normals of both input shapes
    before investigating other causes. Inverted normals are the most common
    cause of unexpected Boolean results in mesh-converted geometry.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a shape (illustrative — in practice this would be a mesh-converted shape)
original = doc.addObject("Part::Box", "Original")
original.Length = 10; original.Width = 10; original.Height = 10
doc.recompute()

# Reverse all face normals
reversed_shape = original.Shape.reversed()
feat = doc.addObject("Part::Feature", "Reversed")
feat.Shape = reversed_shape
doc.recompute()

# Normals are now pointing inward — only useful for fixing an already-inverted shape
print(f"Shape type: {feat.Shape.ShapeType}")
```

---

## See also

- [Shape from Mesh](shape-from-mesh.md) — convert mesh to Part shell (common source of inverted normals)
- [Convert to Solid](convert-to-solid.md) — close a shell to a solid after fixing normals
- [Check Geometry](check-geometry.md) — diagnose normal orientation issues
- [Refine Shape](refine-shape.md) — clean up topology after normal correction
- [Part Workbench](../index.md) — workbench overview
