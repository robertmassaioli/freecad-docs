# Check Geometry

> **In one sentence:** Analyse a Part shape for OCCT validity errors — finding
> open shells, invalid edges, self-intersections, and other topology problems
> that would cause downstream failures.

---

## Overview

**Workbench:** Part · **Menu:** Part → Check Geometry  
**Toolbar:** Part Analysis · **Shortcut:** none

Check Geometry runs OCCT's built-in shape analysis tools on the selected shape
and reports any validity issues. It distinguishes between errors (will cause
failures) and warnings (may cause problems in some operations).

---

## Intuition

OCCT requires shapes to satisfy strict topological rules: edges must be shared
by exactly two faces, vertices must be at the ends of edges, shells must be
closed if they represent a solid, and so on. Violations of these rules are
called *invalid geometry* and cause failures in Boolean operations, meshing,
export, and analysis.

Check Geometry is the diagnostic tool: run it after every import and after
complex Boolean operations to ensure the shape is healthy before continuing.
Fixing errors early is far cheaper than debugging failures three operations later.

---

## When to use it

- **After importing STEP/IGES/STL** — imported files frequently contain small
  gaps, degenerate edges, or other issues.
- **After Boolean Fragments or Slice** — complex multi-shape Boolean operations
  can occasionally produce invalid topology.
- **After Shape from Mesh** — mesh-to-shell conversion often introduces
  degenerate edges.
- **Before FEA meshing** — invalid shapes cause meshing failures.
- **Before export** — STEP export of invalid shapes produces files that other
  CAD tools reject.

---

## Understanding the results

The Check Geometry panel shows a tree of shape elements. Each element is
marked as:

| Indicator | Meaning |
|-----------|---------|
| **Green / no issue** | Valid |
| **BOPAlgo_InvalidCurveOnSurface** | An edge curve does not lie on its adjacent face surface |
| **Bad edge** | An edge has invalid curve parameterisation |
| **Invalid tolerance** | An entity's tolerance is outside acceptable bounds |
| **Self-intersection** | A face or edge intersects itself |
| **Open shell** | A shell has boundary edges (is not watertight) |

---

## Step-by-step walkthrough

1. **Select the shape** in the model tree.
2. **Part → Check Geometry.**
3. The Check Geometry panel opens at the bottom.
4. Click **Run Check** (or it runs automatically).
5. Expand the tree to see which sub-elements (faces, edges, vertices) have issues.
6. Look up the error type and decide on a repair strategy:
   - **Open shell** → find and fill the gap, or use Convert to Solid after
     closing the boundary.
   - **Invalid curve** → the shape needs repair; use [Defeaturing](defeaturing.md)
     or re-import with different settings.
   - **Self-intersection** → simplify the geometry causing the intersection.

---

## Interpreting "shape is valid" vs "BOP check"

Check Geometry runs two tiers of checks:

1. **OCCT shape validity** — basic topology rules. If this fails, the shape
   will not work in any operation.
2. **BOP (Boolean Operation) check** — stricter requirements needed for Boolean
   operations. A shape can pass tier 1 but fail tier 2.

The "Run BOP check" option enables the stricter tier-2 check. Always enable it
before running Boolean operations.

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
doc.recompute()

# Check validity
shape = box.Shape
is_valid = shape.isValid()
print(f"Shape is valid: {is_valid}")

# Detailed check via Part.BRepCheck
# TODO: verify API — BOPTools may be needed for full BOP check
analyzer = Part.BRepCheck.Analyzer(shape)
analyzer.Init(shape)
is_ok = not analyzer.IsValid()
print(f"BRepCheck passed: {not is_ok}")

# Simpler validity check
import BOPTools.BOPFeatures as BF
# Use Check Geometry tool interactively for comprehensive results
print("Run Part → Check Geometry from the menu for a full report.")
```

---

## Common mistakes and pitfalls

!!! warning "Valid shape ≠ manifold solid"
    A shape can pass the basic validity check but still not be a closed (manifold)
    solid. Always look at the specific topology type — `ShapeType == 'Solid'` and
    `shape.isClosed()` for solids.

!!! warning "Suppressed warnings can still cause failures"
    Some issues flagged as warnings (not errors) in Check Geometry can still
    cause Boolean operation failures. Treat all issues as potential problems.

---

## See also

- [Defeaturing](defeaturing.md) — remove problematic faces to simplify geometry
- [Refine Shape](copy-tools.md#refine-shape) — clean up topology after Boolean operations
- [Shape Builder](shape-builder.md) — manually close shells with missing faces
- [Convert to Solid](mesh-and-solid.md#convert-to-solid) — close shells into solids
