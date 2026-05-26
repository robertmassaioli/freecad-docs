# Additive Prism

> **In one sentence:** Add a solid regular-polygon prism — a hexagonal column,
> triangular pillar, or any n-sided post — directly into a Part Design Body,
> fusing it with any existing material.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Prism  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Additive Prism inserts a parametric prism with a regular polygonal cross-section
into the active Body and fuses it with the existing shape. You specify the number
of polygon sides, the circumscribed radius (centre to vertex), and the height.
Two optional taper angles (FirstAngle and SecondAngle) let you draft the sides to
make the prism into a frustum or a tapered post. Because the polygon is always
regular and centred at the origin, this is the fastest way to add hex bosses,
triangular standoffs, octagonal pillars, and similar repeating features without
constructing a sketch.

---

## Intuition

A prism is simply any flat polygon pushed straight up to form a solid column. A
triangular prism is what a Toblerone box looks like; a hexagonal prism is the
shape of a hex bolt head. The cross-section is always a regular polygon — all sides
and interior angles are equal.

The **Polygon** parameter sets how many sides the polygon has. **Circumradius**
is the distance from the polygon's centre to any of its corners (vertices), not to
the middle of a flat face. For a hex wrench size of 10 mm across-flats, the
circumradius is slightly larger than 5 mm; the exact relationship is:

```
circumradius = (across-flats / 2) / cos(π / n)
```

where `n` is the number of sides. For a hexagon: `circumradius = AF / (2 × cos 30°)`.

The **FirstAngle** and **SecondAngle** properties come from the `PrismExtension`
mixin. They tilt the direction of extrusion away from vertical, effectively
producing a tapered prism. Both default to 0° (straight vertical extrusion).

---

## When to use it

- You need a hex boss, square peg, triangular standoff, or any regular-polygon
  column and don't want to draw and constrain a polygonal sketch.
- You are modelling a bolt head, nut profile, or socket drive feature that must
  be exactly a regular polygon.
- You need a tapered prism (a draft angle on all faces) — set FirstAngle or
  SecondAngle to tilt the extrusion.
- You want to quickly prototype a multi-sided post and vary the number of sides
  parametrically.

---

## When NOT to use it

- **The cross-section is not a regular polygon** (e.g. irregular hexagon, rounded
  corners) — draw the profile in a sketch and use [Pad](pad.md) instead.
- **You need different heights on different sides** — the prism always extrudes to
  a flat top face; use a sketch-based feature for tapered or stepped profiles.
- **You want to cut a prism-shaped hole or socket** — use
  [Subtractive Prism](subtractive-prism.md), which has identical parameters but
  removes material.
- **The polygon has fewer than 3 sides** — FreeCAD enforces a minimum of 3 sides
  and will report an error if Polygon < 3.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body in the document. The prism can be
the very first feature (no existing solid required).

1. Open or create a document and ensure a Body is active. FreeCAD will offer to
   create one automatically if none exists.

2. Activate **Part Design → Additive Primitives → Additive Prism** from the menu,
   or click its icon in the Part Design Modeling Primitives toolbar (expand the
   drop-down if multiple primitives share one button).

3. The task panel opens. Set **Polygon** to the number of sides — 6 for a hex, 3
   for a triangle, 8 for an octagon.

4. Set **Circumradius** to the distance from the polygon's centre to a vertex.

5. Set **Height** to the desired extrusion length.

6. Leave **FirstAngle** and **SecondAngle** at 0° for a straight prism. Set them
   to non-zero values if you need tapered sides (draft angle).

7. Click **OK**. The prism is fused into the Body and appears in the model tree
   as **Prism**.

!!! tip
    To match an ISO hex bolt head, measure the "across-flats" (wrench size) and
    convert to circumradius using `circumradius = AF / (2 × cos(30°)) ≈ AF × 0.5774`.
    For example, an M10 hex head with 17 mm across-flats has a circumradius of
    approximately 9.815 mm.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Polygon | Integer | 6 | Number of sides of the regular polygon cross-section. Minimum 3. |
| Circumradius | Length (mm) | 2.0 | Distance from the polygon centre to a vertex (not to a face midpoint). Must be > 0. |
| Height | Length (mm) | 10.0 | Height of the prism (extrusion length along the local Z-axis). Must be > 0. |
| FirstAngle | Angle (°) | 0.0 | Taper angle in the first direction. Range: −89.99° to +89.99°. Tilts the extrusion direction away from vertical, creating a draft on the lateral faces. |
| SecondAngle | Angle (°) | 0.0 | Taper angle in the second direction, orthogonal to FirstAngle. Range: −89.99° to +89.99°. |

---

## Advanced usage

### Across-flats vs. circumradius

The UI uses circumradius (centre to vertex), but engineering drawings typically
specify "across-flats" (AF, the distance between two parallel faces). The
conversion is:

```
circumradius = AF / (2 × cos(π / n))
```

For common polygon counts:

| Sides | Factor (circumradius = AF × factor) |
|-------|--------------------------------------|
| 3 (triangle) | 1 / (2 × cos 60°) = 1.0000 |
| 4 (square) | 1 / (2 × cos 45°) ≈ 0.7071 (use half-diagonal = AF/2 × √2/2) |
| 6 (hexagon) | 1 / (2 × cos 30°) ≈ 0.5774 |
| 8 (octagon) | 1 / (2 × cos 22.5°) ≈ 0.5412 |

!!! note
    For a square (4 sides) the circumradius equals half the diagonal length, not
    half the side length.

### Tapered prism (draft angles)

Setting FirstAngle to a positive value leans the extrusion in the positive X
direction as it grows in Z, while SecondAngle leans it in the Y direction. Both
operate on the same extrusion by composing the two tilt components. This is
equivalent to a shear-extruded prism and produces laterally tapered faces rather
than the constant vertical faces of the default prism.

Typical use case: a tapered knurled grip or a moulded polygonal boss that needs
a draft angle for de-moulding.

### Combining prism with Pocket for hex socket

A common workflow for a hex socket (inward hex hole):

1. Place an **Additive Box** or **Additive Cylinder** as the outer body.
2. Use **Subtractive Prism** (with Polygon = 6) to cut the socket. Set
   Circumradius to the correct hex size and Height to the socket depth.

---

## Common mistakes and pitfalls

!!! warning "Polygon value is less than 3"
    **Cause:** A polygon with fewer than 3 sides is geometrically impossible.
    FreeCAD will produce an error: "Polygon of prism is invalid, must have 3 or
    more sides".  
    **Fix:** Set Polygon to 3 or higher.

!!! warning "Circumradius value appears correct but the polygon is smaller than expected"
    **Cause:** Circumradius is the centre-to-vertex distance, not the
    centre-to-face (inradius) distance. If you entered an across-flats value
    directly as the circumradius, the prism will be smaller than expected.  
    **Fix:** Convert using the formula above, or verify by measuring across the
    flat faces of the generated solid in the viewport.

!!! warning "Prism base is not flat after setting FirstAngle or SecondAngle"
    **Cause:** When taper angles are non-zero, the lateral faces are no longer
    vertical and the apparent width changes along the height.  
    **Fix:** This is the intended behaviour. If you need a strictly constant cross-
    section, reset FirstAngle and SecondAngle to 0°.

!!! warning "Prism is not joined to the existing Body solid"
    **Cause:** The prism does not overlap the current Body tip, so no fusion is
    performed and the result is topologically disconnected.  
    **Fix:** Move the Body origin or use the Attachment tool to position the prism
    so it at least shares a face or intersects the existing solid.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

prism = doc.addObject("PartDesign::AdditivePrism", "Prism")
body.addObject(prism)

# Default hexagonal prism: 6 sides, circumradius=2, height=10
doc.recompute()

print(f"Volume: {prism.Shape.Volume:.4f} mm³")
# Regular hex with circumradius r, height h: V = (3√3/2) * (r*cos(30°))² * h
# TODO: verify computed volume matches analytic formula
```

### Customising polygon and dimensions

```python
prism.Polygon      = 3      # triangle
prism.Circumradius = 8.0    # mm, centre to vertex
prism.Height       = 15.0   # mm
prism.FirstAngle   = 5.0    # 5° draft in X direction
prism.SecondAngle  = 0.0    # no draft in Y direction
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Polygon` | `App::PropertyIntegerConstraint` | Number of polygon sides. Minimum 3. |
| `Circumradius` | `App::PropertyLength` | Radius from polygon centre to vertex, mm. |
| `Height` | `App::PropertyLength` | Prism height along local Z-axis, mm. |
| `FirstAngle` | `App::PropertyAngle` | Extrusion taper angle in first direction, degrees. Range −89.99° to +89.99°. (from `PrismExtension`) |
| `SecondAngle` | `App::PropertyAngle` | Extrusion taper angle in second direction, degrees. Range −89.99° to +89.99°. (from `PrismExtension`) |

---

## See also

- [Subtractive Prism](subtractive-prism.md) — same parameters, cuts a polygonal cavity
- [Additive Cylinder](additive-cylinder.md) — circular cross-section column primitive
- [Additive Box](additive-box.md) — rectangular box primitive
- [Pad](pad.md) — extrude a custom sketch for non-regular polygon cross-sections
