# External Geometry (Projection / Intersection / Carbon Copy)

> **In one sentence:** Bring geometry from outside the current sketch into the
> sketch plane as construction references — either projected onto the plane
> (Projection), intersected with the plane (Intersection), or copied wholesale
> from another sketch (Carbon Copy).

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → External Geometry  
**Toolbar:** Sketcher Geometries (External dropdown) · **Shortcut:** `G, X` (Projection) · `G, W` (Carbon Copy)

| Tool | Shortcut | Description |
|------|----------|-------------|
| Projection | `G, X` | Project a 3-D edge onto the sketch plane as a construction edge |
| Intersection | — | Find where a 3-D face or solid intersects the sketch plane |
| Carbon Copy | `G, W` | Copy all geometry and constraints from another sketch into this one |

External geometry appears as **purple** (or yellow, depending on theme) dashed
construction edges. It cannot be modified inside the sketch but acts as a
constraint reference: you can snap to it, make geometry coincident with it, or
use it to drive dimensional constraints.

---

## Intuition

### Projection

Think of shining a light perpendicular to the sketch plane: the shadow of a 3-D
edge on the plane is the projected edge. Projection is how you say "this sketch
element must align with that 3-D feature" — for example, a hole profile that must
be centred on an existing boss, or a pocket outline that must align with an edge
of the solid below.

### Intersection

Instead of projecting onto the plane, Intersection finds where the sketch plane
actually *cuts through* the solid — like a cross-section slice. This produces the
exact boundary geometry of the solid at the sketch plane's position.

### Carbon Copy

Carbon Copy is a full sketch import: all geometry and constraints from the source
sketch appear inside the current sketch as editable elements. Unlike Projection,
the result is regular geometry (not purple construction edges) that can be extended
and constrained independently.

---

## When to use them

- **Projection:** You need a hole on a pad's top face that aligns with a circular
  edge on a lower face — project the lower circle, then make your new circle
  coincident with the projected centre.
- **Projection:** You want to sketch a pocket that stops at a rib — project the
  rib's top edge as a reference line.
- **Intersection:** You are sketching on a cross-section plane and need the exact
  solid boundary at that level.
- **Carbon Copy:** You want to reuse the profile of one sketch (e.g. a complex
  gasket outline) in a second sketch for a matching feature.

## When NOT to use them

- **Don't rely on external geometry as the actual profile edges** — they are
  construction references. The profile you hand to Part Design must be drawn using
  Sketcher tools (lines, arcs, etc.), not the purple external edges.
- **Don't use Carbon Copy when you want a parametric link** — Carbon Copy produces
  an independent copy; changes to the source sketch do not propagate.
  Use a [ShapeBinder](../../part-design/tools/shape-binder.md) if you need a live
  parametric link.

---

## Step-by-step walkthrough

### Projection

1. **Open a sketch** attached to a face of an existing solid.
2. **Press `G, X`** or activate **Projection** from the External dropdown.
3. **Click a 3-D edge** in the viewport (an edge of the solid, an existing circle,
   a datum line, etc.). The edge projects onto the sketch plane as a dashed purple
   construction edge.
4. **Use the projected edge as a constraint reference:** apply [Coincident](constraint-coincident.md)
   between a sketch vertex and a point on the projection, or [Point on Object](constraint-point-on-object.md)
   to force a sketch edge to touch it.
5. Press ++esc++ to exit Projection mode.

### Intersection

1. Open a sketch.
2. Activate **Intersection** from the External dropdown.
3. **Click a face or solid body.** FreeCAD computes where the sketch plane slices
   through the selected geometry and draws the intersection as a purple construction
   edge.
4. Use the intersection as a reference for constraints.

### Carbon Copy

1. Open a sketch.
2. **Press `G, W`** or activate **Carbon Copy** from the External dropdown.
3. **Click another sketch** in the Model tree or viewport. All its geometry and
   constraints are copied into the current sketch as regular (non-purple) elements.
4. The copied geometry has its own independent DoF and can be constrained freely.

!!! tip
    Carbon Copy is particularly useful for producing two matching sketches (e.g.
    top and bottom profiles for a Loft) that share the same shape. Draw one, then
    Carbon Copy it into the second sketch on a different plane.

---

## Parameters

### Projection / Intersection

| Parameter | Type | Description |
|-----------|------|-------------|
| Selected edge / face | 3-D geometry reference | What to project or intersect. A parametric link is maintained — if the 3-D geometry moves, the projection updates. |

### Carbon Copy

| Parameter | Type | Description |
|-----------|------|-------------|
| Source sketch | Sketch object | The sketch whose geometry is copied. No live link is maintained after the copy. |

---

## External geometry in depth

### Projection vs Intersection — choosing the right one

The key difference is the *relationship* between the sketch plane and the selected geometry:

#### Projection

The 3-D edge is **not necessarily in the sketch plane**. FreeCAD drops a perpendicular from each point of the 3-D edge onto the sketch plane and connects the resulting 2-D points.

**Use Projection when:**
- You want to align a sketch feature with an edge that is below, above, or angled relative to the sketch.
- You need the 2-D "shadow" of a 3-D curve.

**Watch out for:** Projected edges of curved 3-D paths (helices, swept edges) may produce distorted 2-D shapes that are hard to work with as constraint references.

#### Intersection

The sketch plane **cuts through** the selected face or solid. The result is the exact cross-sectional boundary at that level.

**Use Intersection when:**
- The sketch plane passes through the solid and you need its exact cross-section boundary.
- You are creating a sketch that will cut a pocket into an existing body and the pocket must follow the exact boundary of the solid at that level.

**Watch out for:** If the sketch plane does not intersect the selected geometry at all, Intersection produces nothing. Check that the sketch plane actually passes through the face or body.

---

## Advanced usage

### Toggling projection mode

Starting in FreeCAD 1.0, Projection and Intersection are separate menu/toolbar
entries. In earlier versions a single "External Geometry" tool was used for both.

### Detaching external geometry

External geometry creates a parametric reference: if the referenced 3-D edge
moves (because a Pad height changed), the purple projection updates automatically.
This is usually desirable. To break the link (convert to a static snapshot), select
the external edge in the sketch and change it to a regular geometry element.

---

## Common mistakes and pitfalls

!!! warning "Sketch becomes invalid when the referenced edge disappears"
    **Cause:** An external geometry reference points to a face or edge that was
    removed by a topology change (renamed face after a fillet was added).  
    **Fix:** Select the broken external edge (shown in red), delete it, and re-add
    the projection pointing to the correct new edge. This is the well-known
    "topological naming problem" — Part Design's ShapeBinder is more robust for
    long-term references.

!!! warning "Carbon Copy produces conflicting constraints"
    **Cause:** The copied sketch had constraints fixing elements to its own local
    origin; in the new sketch those constraints now conflict with other geometry.  
    **Fix:** After Carbon Copy, open the sketch and remove or reapply the origin
    constraints to suit the new context.

!!! warning "Projected edge appears but cannot be selected for Coincident"
    **Cause:** The external edge is a closed curve (circle, ellipse) and Coincident
    requires a specific vertex, but the tool is picking the edge body rather than
    a point on it.  
    **Fix:** Use [Point on Object](constraint-point-on-object.md) to constrain a
    sketch vertex to lie on the projected circle/arc, rather than Coincident.

---

## Python API

### Projection

```python
import FreeCAD as App

doc = App.activeDocument()
sketch = doc.getObject("Sketch")
pad = doc.getObject("Pad")

# Project Face1 / Edge1 of a Pad onto the sketch
sketch.addExternal("Pad", "Edge1")
doc.recompute()

# The projected edge appears at a negative GeoId (e.g. -3)
# Use it in constraints: sketch.addConstraint(Sketcher.Constraint("Coincident", 0, 1, -3, 1))
```

### Carbon Copy

```python
# Carbon Copy is a GUI command; equivalent Python would be:
import FreeCAD as App
import Part

source = doc.getObject("SketchSource")
target = doc.getObject("SketchTarget")

for geo in source.Geometry:
    target.addGeometry(geo.copy(), False)

doc.recompute()
# Note: constraints are NOT copied by this approach; use the GUI command.
```

---

## See also

- [New Sketch / Edit Sketch](new-sketch.md) — creating the sketch to receive external geometry
- [Validate Sketch](validate-sketch.md) — fix missing constraints after Carbon Copy
- [Coincident](constraint-coincident.md) — align sketch geometry to a projected point
- [Point on Object](constraint-point-on-object.md) — constrain a point onto a projected edge
- [Shape Binder](../../part-design/tools/shape-binder.md) — live parametric reference to 3-D geometry (more robust than external geometry for complex models)
