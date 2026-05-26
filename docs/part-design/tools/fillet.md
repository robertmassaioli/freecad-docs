# Fillet

> **In one sentence:** Round sharp edges of a solid by replacing them with a smooth
> circular arc of a specified radius.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Dress-Up Features → Fillet  
**Toolbar:** Part Design Dress-Up Features · **Shortcut:** none

Fillet selects one or more edges of the active Body's current solid and replaces
each sharp edge with a smoothly curved surface whose cross-section is a circular
arc. The operation modifies the existing solid in-place — it does not add a
separate feature body — and the result remains parametric: changing the radius
updates the shape immediately.

---

## Intuition

Think of sanding the corner of a wooden block. The sharp 90-degree edge is
replaced by a smooth curved surface. The radius of that curve is exactly what
you set in the Fillet dialog: a small radius gives a barely-there bevel, while a
large radius gives a sweeping curve that eats noticeably into both adjacent faces.

The key constraint is that the radius cannot exceed half the width of either
adjacent face — if the fillet circle is too large to fit between the two faces it
touches, FreeCAD (and the underlying OCCT kernel) cannot construct a valid solid.
A common sign that the radius is too large is the error "Fillet not possible on
selected shapes."

The tool works on any combination of straight and curved edges, including edges
that form a continuous chain around a pocket or boss. When you select a face
instead of individual edges, FreeCAD fillets all edges that bound that face.

---

## When to use it

- You want to remove sharp corners from a machined part to reduce stress
  concentrations.
- You are modelling a plastic injection moulded part, where all internal and
  external corners need a blend radius for mould-release and structural reasons.
- You need a cosmetic rounded edge on a consumer product model.
- You want to fillet all edges of a solid at once (use the *Use All Edges*
  option).
- You are preparing a model for FEA and need chamfer-free, smooth transitions at
  stress-critical edges.

---

## When NOT to use it

- **You want a flat, angled cut rather than a curved blend** — use
  [Chamfer](chamfer.md) instead.
- **The edge you want to round is on a sketch, not on a solid** — round the
  sketch geometry with a Sketcher fillet or constrained arc before padding it.
- **You need a blend between two bodies** — use Part workbench Boolean operations
  followed by a Part Fillet; PartDesign Fillet only operates on features inside
  the current Body.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one solid feature (a Pad,
Revolution, or similar). The solid must have edges to fillet.

1. **Select the edges to fillet.** In the 3-D viewport or the model tree,
   click one or more edges of the solid. Holding ++ctrl++ lets you add edges to
   the selection. You may also select a face to auto-select all its bounding
   edges. Alternatively, skip pre-selection and use the task panel's reference
   list after opening the tool.

2. **Open Fillet.** Go to **Part Design → Dress-Up Features → Fillet**, or click
   the Fillet button in the *Part Design Dress-Up Features* toolbar.

3. **The task panel opens** showing a *Fillet radius* field and a list of
   selected edges.

    !!! warning "No edges in the list"
        If the reference list is empty, click **Add Reference** in the task panel
        and then click each edge in the viewport.

4. **Set the radius.** Type a value in the *Fillet radius* field or use the
   spin box. The preview updates in real time. A radius of `1 mm` is the default.

5. **Optionally enable Use All Edges.** Tick *Use all edges* to apply the fillet
   to every edge of the solid. When this is on, the edge list is ignored.

6. **Click OK** to confirm. The Fillet feature appears in the model tree below
   the feature it modifies.

!!! tip
    Right-click an edge in the reference list to remove it. Right-click the
    viewport and choose *Add all edges* to populate the list with every edge on
    the solid.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Fillet radius | Length | `1 mm` | The radius of the circular arc that replaces each selected edge. Must be greater than zero. Valid range: `> 0` to the maximum length that fits on the narrowest adjacent face. |
| Use all edges | Toggle | off | When enabled, all edges of the base solid are filleted; the reference list is ignored. |
| References (edge list) | Sub-element links | — | The individual edges or faces whose edges will be rounded. Active only when *Use all edges* is off. |

---

## Advanced usage

### Continuous-edge propagation

FreeCAD automatically includes edges that are tangentially continuous with the
ones you select. If you select one edge of a smoothly rounded pocket, all
tangent-connected edges in that chain are filleted together. This is the
`getContinuousEdges` behaviour from the source, and it prevents OCCT from
failing at tangent junctions.

### Variable-radius fillet

PartDesign Fillet applies a constant radius to all selected edges. If you need
a variable radius along a single edge (a tapered fillet), use the Part
workbench **Fillet** feature (`Part::Fillet`), which exposes OCCT's variable-
radius API. <!-- TODO: verify that PartDesign::Fillet does not expose variable
radius in FreeCAD 1.x -->

### Fillet ordering and feature tree position

Fillets are applied after all other features above them in the tree. Applying
a large fillet before a pocket that cuts through the filleted face can produce
unexpected results. As a rule of thumb: add fillets and chamfers last, after
all material-adding and material-removing operations are complete.

---

## Common mistakes and pitfalls

!!! warning "Fillet not possible on selected shapes"
    **Cause:** The radius is too large for the geometry. The fillet arc cannot
    fit between the two faces adjacent to the edge — it would extend past one of
    them.  
    **Fix:** Reduce the radius, or select fewer edges and fillet them at a
    smaller radius first.

!!! warning "Resulting shape is null"
    **Cause:** OCCT produced an empty result, typically because the combination
    of edges and radius creates self-intersecting geometry.  
    **Fix:** Try filleting edges one at a time, using smaller radii, or avoid
    filleting edges that meet at very acute angles.

!!! warning "Result has multiple solids"
    **Cause:** The fillet operation split the solid into disconnected pieces
    (rare but possible on thin features).  
    **Fix:** Enable *Allow Compound* in the active Body settings, or redesign
    the geometry to avoid the problematic edge combination.

!!! warning "Empty fillet created"
    **Cause:** You clicked OK with no edges in the reference list and *Use all
    edges* was off.  
    **Fix:** Re-open the feature (double-click it in the model tree), click *Add
    Reference*, and select at least one edge.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign
import Sketcher
import Part

doc = App.newDocument()

# Create a body with a simple box (padded rectangle)
body = doc.addObject("PartDesign::Body", "Body")

sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
body.addObject(sketch)
sketch.addGeometry(Part.LineSegment(App.Vector(0,0,0), App.Vector(10,0,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(10,0,0), App.Vector(10,10,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(10,10,0), App.Vector(0,10,0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(0,10,0), App.Vector(0,0,0)), False)
sketch.addConstraint(Sketcher.Constraint("Coincident", 0,2, 1,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 1,2, 2,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 2,2, 3,1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 3,2, 0,1))
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = sketch
pad.Length = 10.0
doc.recompute()

# Add a fillet to all edges
fillet = doc.addObject("PartDesign::Fillet", "Fillet")
body.addObject(fillet)
fillet.Base = (pad, [])          # base feature; edge list populated separately
fillet.Radius = 1.0              # 1 mm radius  # TODO: verify edge selection API
fillet.UseAllEdges = True
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Radius` | `App::PropertyQuantityConstraint` | The fillet radius in document length units. Minimum: `0`, step: `0.1`. |
| `UseAllEdges` | `App::PropertyBool` | When `True`, all edges of the base shape are filleted, overriding any edge list in `Base`. Default: `False`. |
| `Base` | `App::PropertyLinkSub` | Link to the base feature and the list of selected sub-edges or sub-faces. Inherited from `DressUp`. |
| `SupportTransform` | `App::PropertyBool` | Allow the dress-up to follow pattern transformations. Inherited from `DressUp`. |

---

## See also

- [Chamfer](chamfer.md) — flat angled cut instead of a curved blend
- [Draft](draft.md) — taper faces for mould release
- [Thickness](thickness.md) — hollow out a solid by a uniform wall thickness
- [Pad](pad.md) — the most common feature to fillet after creation
