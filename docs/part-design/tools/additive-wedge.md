# Additive Wedge

> **In one sentence:** Add a solid wedge — a box whose top face is independently
> sized and offset from its bottom face — directly into a Part Design Body, fusing
> it with any existing material.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Primitives → Additive Wedge  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Additive Wedge inserts a parametric wedge solid into the active Body and fuses it
with the existing shape. A wedge is defined by two rectangular faces — one at the
bottom (Y = Ymin) and one at the top (Y = Ymax) — each described by its own X and
Z extents. Because the top face can be independently sized and horizontally offset
from the bottom face, you can create classic tapered wedges, ramps, pyramids with
rectangular bases, and any general linear solid that transitions between two
rectangles. No sketch is required.

---

## Intuition

Think of a wedge as a box that you can squash, tilt, or taper. The bottom face
sits in the XZ-plane at height Y = Ymin. It spans from `Xmin` to `Xmax` along X,
and from `Zmin` to `Zmax` along Z. The top face sits at Y = Ymax. It has its own
independent extents: `X2min` to `X2max` along X, and `Z2min` to `Z2max` along Z.

Connecting these two rectangles with straight lines gives you the wedge solid.
If the top face is the same size and position as the bottom face you get a
rectangular box. If the top face is smaller and centred you get a truncated
pyramid. If the top face is zero-sized (X2min = X2max and Z2min = Z2max at the
same value) you get a full pyramid that comes to an edge or a point at the top.

Because the top face can be offset horizontally — not just scaled — you can
produce ramp shapes where the top face is the same size as the bottom but slid
along X or Z. This gives a true wedge/ramp, useful for support brackets, cam
shapes, and tapered spacers.

The ten parameters fully describe the geometry; there is no redundancy. All
coordinates are in the local coordinate system of the Body origin.

---

## When to use it

- You need a simple ramp, wedge, or shim — a shape that is thick on one side and
  thin on the other.
- You are modelling a door stop, tapered cleat, bracket gusset, or any part with
  a linearly varying profile.
- You need a pyramid or truncated pyramid (frustum with a rectangular base) as a
  design element or stand-alone solid.
- You want to block in a draft-angle tapered boss without constructing a lofted
  sketch, and the cross-section transitions between two rectangles.

---

## When NOT to use it

- **The cross-section is not a rectangle at both ends** — use
  [Additive Loft](additive-loft.md) with custom profile sketches for arbitrary
  transitions.
- **You need a wedge-shaped cut rather than added material** — use
  [Subtractive Wedge](subtractive-wedge.md), which has identical parameters but
  removes material.
- **The taper is conical or elliptical** — use [Additive Cone](additive-cone.md)
  or [Additive Ellipsoid](additive-ellipsoid.md) for rounded shapes.
- **The dimensions are driven by relations to other geometry** — if you need a
  wedge whose dimensions are derived from existing faces or edges, a lofted profile
  sketch with constraints will be more maintainable.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body in the document. The wedge can be
the very first feature (no existing solid required).

1. Open or create a document and ensure a Body is active. FreeCAD will offer to
   create one automatically if none exists.

2. Activate **Part Design → Additive Primitives → Additive Wedge** from the menu,
   or click its icon in the Part Design Modeling Primitives toolbar (expand the
   drop-down if needed).

3. The task panel opens. Understand the coordinate layout before entering values:
   - **Bottom face** (at Y = Ymin): spans `Xmin` → `Xmax` in X and `Zmin` → `Zmax` in Z.
   - **Top face** (at Y = Ymax): spans `X2min` → `X2max` in X and `Z2min` → `Z2max` in Z.

4. Set the **bottom face** extents: enter values for `Xmin`, `Xmax`, `Zmin`,
   and `Zmax`. For a bottom face that starts at the origin with dimensions
   10 × 10 mm, use Xmin=0, Xmax=10, Zmin=0, Zmax=10.

5. Set the **height**: enter `Ymin` (typically 0) and `Ymax` (the height of the
   wedge, e.g. 10).

6. Set the **top face** extents: enter `X2min`, `X2max`, `Z2min`, and `Z2max`.
   For a simple tapered ramp, set these to the same X range but shift them along
   Z: e.g. X2min=2, X2max=8, Z2min=2, Z2max=8 produces a centred truncated
   pyramid.

7. Click **OK**. The wedge is fused into the Body and appears in the model tree
   as **Wedge**.

!!! tip
    To create a classic right-angled ramp (thick at one end, pointed at the other),
    set the top face to a zero-width edge: X2min = X2max (e.g., both = 5 for the
    midpoint). The wedge will have a sharp ridge at the top instead of a flat face.

---

## Parameters

All ten parameters are `App::PropertyDistance` values in mm.

The bottom face is at Y = Ymin. The top face is at Y = Ymax.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Xmin | Distance (mm) | 0.0 | Minimum X coordinate of the **bottom** face. |
| Ymin | Distance (mm) | 0.0 | Y coordinate of the bottom face (the base height). |
| Zmin | Distance (mm) | 0.0 | Minimum Z coordinate of the **bottom** face. |
| X2min | Distance (mm) | 2.0 | Minimum X coordinate of the **top** face. Can be offset from Xmin. |
| Z2min | Distance (mm) | 2.0 | Minimum Z coordinate of the **top** face. Can be offset from Zmin. |
| Xmax | Distance (mm) | 10.0 | Maximum X coordinate of the **bottom** face. Must satisfy Xmax > Xmin. |
| Ymax | Distance (mm) | 10.0 | Y coordinate of the top face (the top height). Must satisfy Ymax > Ymin. |
| Zmax | Distance (mm) | 10.0 | Maximum Z coordinate of the **bottom** face. Must satisfy Zmax > Zmin. |
| X2max | Distance (mm) | 8.0 | Maximum X coordinate of the **top** face. X2max ≥ X2min required (can equal X2min for a ridge). |
| Z2max | Distance (mm) | 8.0 | Maximum Z coordinate of the **top** face. Z2max ≥ Z2min required. |

**Validity constraints enforced by FreeCAD:**

- `Xmax − Xmin > 0` (bottom face has positive X width)
- `Ymax − Ymin > 0` (wedge has positive height)
- `Zmax − Zmin > 0` (bottom face has positive Z depth)
- `X2max − X2min ≥ 0` (top face X span can be zero — a ridge — but not negative)
- `Z2max − Z2min ≥ 0` (top face Z span can be zero but not negative)

---

## Advanced usage

### Creating a rectangular pyramid

Set the top face to a degenerate point or zero-size rectangle:
```
X2min = X2max = (Xmin + Xmax) / 2
Z2min = Z2max = (Zmin + Zmax) / 2
```
This produces a four-sided pyramid with a rectangular base. For a perfect square
pyramid, ensure the bottom face is a square (Xmax − Xmin = Zmax − Zmin) and place
the apex at the centre.

### Creating an offset ramp (door stop / wedge shim)

A true ramp with the top face shifted along Z:
```
Xmin=0, Xmax=10, Zmin=0, Zmax=10, Ymin=0, Ymax=10
X2min=0, X2max=10, Z2min=5, Z2max=10
```
Here the bottom face is a 10×10 square and the top face is a 10×5 rectangle
shifted to the far Z end, producing a ramp that is flat on one side and sloped on
the other.

### Asymmetric taper

Both the X and Z dimensions of the top face can be independently offset and sized,
allowing a trapezoid frustum where each pair of opposite faces has a different taper
angle. This level of control is unique to the Wedge — no other primitive exposes
this many independent face coordinates.

### Negative coordinates are allowed

All ten parameters accept negative values. Setting Xmin < 0 and Xmax > 0 places
the bottom face straddling the Y-axis, which is useful when you want the wedge
centred on a symmetry plane.

---

## Common mistakes and pitfalls

!!! warning "Error: delta x of wedge too small"
    **Cause:** `Xmax − Xmin` is zero or negative — the bottom face has no width.  
    **Fix:** Ensure `Xmax > Xmin`. The same logic applies to Y (Ymax > Ymin) and
    Z (Zmax > Zmin).

!!! warning "Error: delta x2 of wedge is negative"
    **Cause:** `X2max < X2min` — the top face X range is inverted.  
    **Fix:** Ensure `X2max ≥ X2min` and `Z2max ≥ Z2min`. A value of zero is
    allowed (produces a top edge or point); negative values are not.

!!! warning "Wedge looks like a box — the taper has no visible effect"
    **Cause:** The top face (X2 and Z2) extents happen to match the bottom face
    extents exactly.  
    **Fix:** Change X2min, X2max, Z2min, or Z2max to values different from their
    bottom-face counterparts to introduce the taper.

!!! warning "Unexpected shape after changing only one parameter"
    **Cause:** Because all ten parameters interact to define both face rectangles
    and the height, changing one (for example, Xmax) without adjusting the top
    face (X2max) may produce an asymmetric shape unintentionally.  
    **Fix:** Think in terms of "what should the bottom face look like" and "what
    should the top face look like" and set all four bounds for each face
    independently.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

wedge = doc.addObject("PartDesign::AdditiveWedge", "Wedge")
body.addObject(wedge)

# Default wedge: bottom 10×10, top 6×6 (offset 2 on each side), height 10
# Xmin=0, Ymin=0, Zmin=0, X2min=2, Z2min=2
# Xmax=10, Ymax=10, Zmax=10, X2max=8, Z2max=8
doc.recompute()

print(f"Volume: {wedge.Shape.Volume:.4f} mm³")
# TODO: verify computed volume
```

### Creating a simple ramp

```python
wedge.Xmin  = 0.0
wedge.Xmax  = 20.0
wedge.Ymin  = 0.0
wedge.Ymax  = 5.0
wedge.Zmin  = 0.0
wedge.Zmax  = 10.0
# Top face: same X width, shifted to far Z edge (a ramp rising from Zmin to Zmax)
wedge.X2min = 0.0
wedge.X2max = 20.0
wedge.Z2min = 10.0   # top face collapses to a line at Zmax
wedge.Z2max = 10.0
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Xmin` | `App::PropertyDistance` | Minimum X of bottom face, mm. |
| `Ymin` | `App::PropertyDistance` | Y coordinate of bottom face (base level), mm. |
| `Zmin` | `App::PropertyDistance` | Minimum Z of bottom face, mm. |
| `X2min` | `App::PropertyDistance` | Minimum X of top face, mm. |
| `Z2min` | `App::PropertyDistance` | Minimum Z of top face, mm. |
| `Xmax` | `App::PropertyDistance` | Maximum X of bottom face, mm. |
| `Ymax` | `App::PropertyDistance` | Y coordinate of top face (wedge height), mm. |
| `Zmax` | `App::PropertyDistance` | Maximum Z of bottom face, mm. |
| `X2max` | `App::PropertyDistance` | Maximum X of top face, mm. |
| `Z2max` | `App::PropertyDistance` | Maximum Z of top face, mm. |

---

## See also

- [Subtractive Wedge](subtractive-wedge.md) — same parameters, cuts a wedge-shaped cavity
- [Additive Box](additive-box.md) — simpler rectangular box (uniform top and bottom face)
- [Additive Loft](additive-loft.md) — blend between arbitrary sketch profiles for non-rectangular transitions
- [Additive Prism](additive-prism.md) — regular-polygon column primitive
- [Pad](pad.md) — extrude any sketch profile for full shape control
