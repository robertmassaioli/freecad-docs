# Subtractive Prism

> **In one sentence:** Cut a regular n-sided prismatic cavity — a hexagonal,
> triangular, or other polygonal bore — out of an existing Part Design Body solid.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Prism  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Subtractive Prism removes a regular polygonal prism volume from the current tip
of the active Body using a Boolean cut. The prism is defined by the number of
polygon sides, a circumradius (centre to vertex), and a height. Two optional draft
angles (FirstAngle and SecondAngle, from the `PrismExtension`) allow the prism's
walls to taper, producing angled bores or pocket entries. Because no sketch is
required, it is the fastest way to cut a hex socket, a triangular locating pocket,
or any other regular-polygon bore.

---

## Intuition

A drill bit removes a cylindrical bore. A square broach removes a square bore.
Subtractive Prism is the parametric equivalent of a broaching tool with an
adjustable number of sides: you specify how many sides (3 = triangle, 6 = hex,
etc.), how wide the polygon is (circumradius), and how deep to cut (height), and
FreeCAD removes the matching prism.

The circumradius is measured from the polygon's centre to its vertices — this is
the same convention as an Allen key size in metric tooling. All sides and all
interior angles of the polygon are equal; you cannot create an irregular polygon
with this tool.

The optional FirstAngle and SecondAngle parameters tilt the prism's direction
vector away from vertical (the local Z-axis). A non-zero FirstAngle tilts in the
X-direction; a non-zero SecondAngle tilts in the Y-direction. This is useful for
draft angles on injection-moulded features or for oblique bores, but for most uses
both angles should remain at `0°`.

---

## When to use it

- You need a hexagonal socket, Allen-key hole, or wrench flat in a solid without
  drawing a hexagonal sketch and pocketing it.
- You need a triangular, square, pentagonal, or other regular-polygon pocket or
  through-bore.
- You want a lightly tapered polygonal bore (mould draft) by setting a small
  FirstAngle or SecondAngle.
- You are iterating quickly on the number of sides and size of a recessed
  feature.

---

## When NOT to use it

- **The pocket profile is not a regular polygon** — use [Pocket](pocket.md) with a
  custom sketch for any irregular shape.
- **You need to add a polygonal boss** — use [Additive Prism](additive-prism.md)
  instead; it has identical parameters but adds material.
- **The bore axis must follow a curve** — use
  [Subtractive Pipe](subtractive-pipe.md) to sweep a polygonal profile along a
  path.
- **No solid exists in the Body** — subtractive primitives require an existing
  base feature. FreeCAD will display an error if the Tip is empty.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body with at least one existing solid
feature (the Tip must be non-empty).

1. Ensure the Body you want to modify is active. If it is not, double-click it in
   the model tree.

2. Activate **Part Design → Subtractive Primitives → Subtractive Prism** from the
   menu, or click its icon in the Part Design Modeling Primitives toolbar. If
   multiple primitives share one toolbar button, expand the drop-down and select
   **Subtractive Prism**.

3. The task panel opens showing the prism parameters. A preview of the prism
   appears in the viewport.

4. Set **Polygon** to the number of sides. A hexagonal socket requires `6`; a
   square drive requires `4`. The minimum valid value is `3`.

5. Set **Circumradius** to the distance from the polygon's centre to its outermost
   vertices. For a standard M5 hex socket the circumradius is typically 2.87 mm
   <!-- TODO: verify hex socket sizing convention -->.

6. Set **Height** to the required pocket depth (or total length for a through-bore).

7. Leave **FirstAngle** and **SecondAngle** at `0°` for a straight vertical bore.
   Set a small positive value (e.g. `1°` to `3°`) for a draft taper.

8. Click **OK**. FreeCAD performs the Boolean cut and the Body shape is updated.
   The feature appears in the model tree as **Prism**.

!!! tip
    For a centred hexagonal socket, the circumradius equals the hex key size
    divided by the cosine of 30°: `r = hex_size / cos(30°)`. For example, a
    4 mm hex key requires a circumradius of approximately 4.62 mm. In practice,
    add a small clearance (0.1–0.2 mm) for a functional fit.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Polygon | Integer | 6 | Number of sides of the regular polygon. Minimum value: 3. A value of 6 produces a hexagonal prism. |
| Circumradius | Length (mm) | 2.0 | Radius from the polygon centre to a vertex (circumscribed circle radius). Controls the overall size of the cavity. Must be > 0. |
| Height | Length (mm) | 10.0 | Depth of the prismatic cavity along the local Z-axis. Must be > 0. |
| FirstAngle | Angle (°) | 0.0 | Tilt of the prism axis in the X-direction. Range: −89.99° to +89.99°. Zero gives a straight vertical bore. |
| SecondAngle | Angle (°) | 0.0 | Tilt of the prism axis in the Y-direction. Range: −89.99° to +89.99°. Zero gives a straight vertical bore. |

---

## Advanced usage

### Draft angles for moulded parts

Setting a small FirstAngle (e.g. `2°`) causes the prism walls to taper inward as
they descend, mimicking the draft angle required for parts that must release from
an injection-mould tool. Both FirstAngle and SecondAngle can be combined; the
resulting bore axis is a vector sum of the two tilts.

### High polygon counts

Setting Polygon to a large value (20, 36, etc.) approximates a circular bore.
For polygon counts above approximately 12, the visual difference from a true
circle is negligible at typical engineering tolerances. If you need an exact
circle, use [Subtractive Cylinder](subtractive-cylinder.md) instead.

### Subtractive Prism vs. Pocket with a regular polygon sketch

A Subtractive Prism is equivalent to a [Pocket](pocket.md) operation on a
fully-constrained regular polygon sketch. For simple, regular polygons the
primitive is faster because no sketch is required. Use Pocket when you need to
position the polygon relative to existing geometry (e.g. constrained to a bolt
circle), when you need a rounded vertex polygon, or when the shape is not regular.

---

## Common mistakes and pitfalls

!!! warning "Cannot subtract primitive feature without base feature"
    **Cause:** The Body's Tip is empty — no additive feature has been added yet.  
    **Fix:** Add at least one additive feature (Pad, Additive Box, etc.) to the
    Body before applying any subtractive primitive.

!!! warning "Polygon value below 3 — prism is invalid"
    **Cause:** FreeCAD requires at least 3 sides to form a valid polygon. Setting
    Polygon to 1 or 2 causes an execute error.  
    **Fix:** Set Polygon to 3 (triangle) or higher. The source enforces a minimum
    of 3 sides.

!!! warning "Prism cavity is centred at the wrong location"
    **Cause:** The prism is positioned at the Body origin by default, which may not
    be on the face where the socket is needed.  
    **Fix:** Use the Attachment tool to align the prism's local coordinate system
    to the correct face and reference geometry before clicking OK. Alternatively,
    use a [Pocket](pocket.md) with a positioned sketch for more precise control.

!!! warning "Vertex orientation of the hexagon does not match the drawing"
    **Cause:** The prism polygon always starts with one vertex along the local
    X-axis. If the hexagon must be rotated (e.g. flat-side-up), the primitive does
    not offer a rotation parameter.  
    **Fix:** Use the Attachment tool's offset rotation field to rotate the
    primitive's local coordinate system to the required orientation, or use a
    Pocket with a rotated sketch.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

# Create a base block to cut into
box = doc.addObject("PartDesign::AdditiveBox", "Box")
body.addObject(box)
box.Length = 20.0
box.Width = 20.0
box.Height = 15.0
doc.recompute()

# Cut a hex socket (6-sided prism)
prism = doc.addObject("PartDesign::SubtractivePrism", "Prism")
body.addObject(prism)

# Defaults: Polygon=6, Circumradius=2, Height=10, FirstAngle=0, SecondAngle=0
doc.recompute()

print(f"Volume: {prism.Shape.Volume:.4f} mm³")
# TODO: verify computed volume
```

### Customising the prism

```python
prism.Polygon      = 6      # hexagonal socket
prism.Circumradius = 4.62   # circumradius for a 4 mm hex key (4 / cos(30°))
prism.Height       = 8.0    # depth of socket
prism.FirstAngle   = 0.0    # no taper in X
prism.SecondAngle  = 0.0    # no taper in Y
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Polygon` | `App::PropertyIntegerConstraint` | Number of polygon sides. Minimum 3. |
| `Circumradius` | `App::PropertyLength` | Centre-to-vertex radius of the polygon, mm. |
| `Height` | `App::PropertyLength` | Depth of the prismatic cavity, mm. |
| `FirstAngle` | `App::PropertyAngle` | Tilt angle in the X-direction, degrees. Range −89.99° to +89.99°. |
| `SecondAngle` | `App::PropertyAngle` | Tilt angle in the Y-direction, degrees. Range −89.99° to +89.99°. |

---

## See also

- [Additive Prism](additive-prism.md) — same parameters, adds a polygonal boss instead of cutting a socket
- [Pocket](pocket.md) — remove material by extruding any closed sketch downward; use for non-regular polygons
- [Subtractive Cylinder](subtractive-cylinder.md) — circular bore; equivalent to Subtractive Prism with a very high polygon count
- [Subtractive Box](subtractive-box.md) — rectangular slot; equivalent to a 4-sided prism but with independent width and length
