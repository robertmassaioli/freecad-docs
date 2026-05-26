# Subtractive Wedge

> **In one sentence:** Cut a tapered box-shaped cavity — a wedge whose top face
> is smaller than or offset from its bottom face — out of an existing Part Design
> Body solid.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Primitives → Subtractive Wedge  
**Toolbar:** Part Design Modeling Primitives · **Shortcut:** none

Subtractive Wedge removes a wedge-shaped volume from the current tip of the
active Body using a Boolean cut. The wedge is an OCCT generalised tapered box
defined by ten coordinates: minimum and maximum extents along X, Y, and Z at the
base level, plus independent minimum and maximum extents for a second rectangular
face at the top. This lets you cut tapered pockets, angled slots, prismatic ramps,
and asymmetric channels that would require complex sketch work to produce any other
way.

---

## Intuition

A wedge is best understood as a box whose top face has been independently resized
and repositioned relative to its bottom face. The bottom face sits in the XZ plane
and is defined by the intervals `[Xmin, Xmax]` and `[Zmin, Zmax]`. The top face,
at height `Ymax`, is independently defined by `[X2min, X2max]` and
`[Z2min, Z2max]`. FreeCAD connects the four corners of the bottom face to the four
corners of the top face with straight edges, producing a solid with six quadrilateral
faces.

Concretely:
- Set all four `*2*` values equal to the corresponding base values for a plain
  rectangular box with no taper.
- Shrink the top face relative to the bottom face (e.g. `X2min > Xmin` and
  `X2max < Xmax`) to get a classic "roof ridge" wedge that comes to a ridge at the
  top.
- Shift the top face sideways (e.g. `X2min = X2max = some_value`) to get a
  parallelogram cross-section — useful for angled entry chamfers or ramp
  pockets.
- Set both top extents to the same value on one axis to get a true sharp edge at
  the top (a proper wedge-shaped taper).

All ten coordinates are independent distances, not widths, so the shape is fully
described by absolute min/max pairs rather than a centre + size.

---

## When to use it

- You need a tapered pocket or chamfered entry — for example, a dovetail relief
  or an angled counterbore entry — where the top opening is a different size from
  the bottom.
- You need to cut a ramp or inclined slot where one end is taller than the other.
- You want an asymmetric tapered cavity that would require a complex lofted pocket
  to create otherwise.
- You are roughing out a complex shape quickly and want to subtract approximate
  wedge volumes before refining with sketch-based operations.

---

## When NOT to use it

- **The cavity is a simple rectangular box** — use [Subtractive Box](subtractive-box.md)
  instead; its Length/Width/Height parameters are easier to reason about for
  untapered slots.
- **You need to add a wedge-shaped boss** — use [Additive Wedge](additive-wedge.md),
  which has identical parameters but fuses material.
- **The taper is on a cylindrical surface** — use [Subtractive Cone](subtractive-cone.md)
  for conical bores.
- **No solid exists in the Body** — subtractive primitives require an existing base
  feature. FreeCAD will display an error if the Tip is empty.

---

## Step-by-step walkthrough

**Prerequisites:** An active Part Design Body with at least one existing solid
feature (the Tip must be non-empty).

1. Ensure the Body you want to modify is active. If it is not, double-click it in
   the model tree.

2. Activate **Part Design → Subtractive Primitives → Subtractive Wedge** from the
   menu, or click its icon in the Part Design Modeling Primitives toolbar. If
   multiple primitives share one toolbar button, expand the drop-down and select
   **Subtractive Wedge**.

3. The task panel opens with the ten wedge coordinate parameters. A preview of the
   default wedge shape appears in the viewport.

4. Set the **base face** coordinates. The base face sits at `Y = Ymin` (default
   `0`): adjust `Xmin`, `Xmax`, `Zmin`, and `Zmax` to define the base rectangle.

5. Set **Ymax** to the height of the wedge (the Y extent of the cavity).

6. Set the **top face** coordinates. The top face sits at `Y = Ymax`: adjust
   `X2min`, `X2max`, `Z2min`, and `Z2max` to define the top rectangle. To produce
   a taper, make these values different from the corresponding base values.

7. Click **OK**. FreeCAD performs the Boolean cut and the Body shape is updated.
   The feature appears in the model tree as **Wedge**.

!!! tip
    To visualise the shape before committing, sketch the desired cross-section on
    paper first. Label the bottom-face corners with the `Xmin/Xmax, Zmin/Zmax`
    values and the top-face corners with `X2min/X2max, Z2min/Z2max`. Then enter
    those values directly — the correspondence is one-to-one.

!!! warning "Ymin must be less than Ymax"
    FreeCAD requires `Ymax - Ymin > 0` for the wedge to have a positive height.
    The default places `Ymin = 0` and `Ymax = 10`. If you change Ymin, ensure
    Ymax remains strictly greater than Ymin.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Xmin | Distance (mm) | 0.0 | Minimum X coordinate of the base face (at Y = Ymin). |
| Ymin | Distance (mm) | 0.0 | Y coordinate of the base face. Typically 0. |
| Zmin | Distance (mm) | 0.0 | Minimum Z coordinate of the base face (at Y = Ymin). |
| X2min | Distance (mm) | 2.0 | Minimum X coordinate of the top face (at Y = Ymax). |
| Z2min | Distance (mm) | 2.0 | Minimum Z coordinate of the top face (at Y = Ymax). |
| Xmax | Distance (mm) | 10.0 | Maximum X coordinate of the base face. `Xmax - Xmin` is the base face width. Must be > Xmin. |
| Ymax | Distance (mm) | 10.0 | Y coordinate of the top face. `Ymax - Ymin` is the wedge height. Must be > Ymin. |
| Zmax | Distance (mm) | 10.0 | Maximum Z coordinate of the base face. `Zmax - Zmin` is the base face depth. Must be > Zmin. |
| X2max | Distance (mm) | 8.0 | Maximum X coordinate of the top face. |
| Z2max | Distance (mm) | 8.0 | Maximum Z coordinate of the top face. |

**Constraints enforced by the source:**

- `Xmax - Xmin` must be > 0 (delta X cannot be zero or negative).
- `Ymax - Ymin` must be > 0 (delta Y cannot be zero or negative).
- `Zmax - Zmin` must be > 0 (delta Z cannot be zero or negative).
- `X2max - X2min` must be ≥ 0 (delta X2 may be zero — top edge collapses to a line).
- `Z2max - Z2min` must be ≥ 0 (delta Z2 may be zero — top edge collapses to a line).

---

## Advanced usage

### Ridge and sharp-edge wedges

Setting the top face to a line (e.g. `X2min = X2max = 5.0`) collapses one
dimension of the top face to a single edge, creating a classic wedge that tapers
to a ridge. Setting both `X2min = X2max` and `Z2min = Z2max` (to the same values)
collapses the top to a single point, giving a pyramid-shaped cavity.

### Parallelogram (shear) wedges

Setting `X2min = Xmin + offset` and `X2max = Xmax + offset` (the same offset for
both) shifts the top face horizontally without changing its size. The result is a
parallelogram cross-section — useful for cutting angled ramp slots where the
cavity walls are slanted but parallel.

### Dovetail approximation

A rough dovetail slot can be approximated by setting the top face wider than the
base face (`X2min < Xmin` and `X2max > Xmax`). This inverted taper undercuts the
base solid. For a true dovetail profile with a specific angle, use a
[Pocket](pocket.md) with a constrained trapezoidal sketch instead.

### Subtractive primitives vs. Pocket

Subtractive Wedge is most useful when the exact cavity dimensions are known as
absolute coordinates (min/max pairs) and when the shape is one of the degenerate
forms described above. For any cavity that must be positioned relative to existing
edges, constrained to a bolt pattern, or defined by angles and dimensions rather
than absolute coordinates, a [Pocket](pocket.md) with a sketch is more precise
and easier to dimension correctly.

---

## Common mistakes and pitfalls

!!! warning "Cannot subtract primitive feature without base feature"
    **Cause:** The Body's Tip is empty — no additive feature has been added yet.  
    **Fix:** Add at least one additive feature (Pad, Additive Box, etc.) to the
    Body before applying any subtractive primitive.

!!! warning "delta x/y/z of wedge too small — execution error"
    **Cause:** One or more of the base face extents is zero or negative (Xmax ≤
    Xmin, Ymax ≤ Ymin, or Zmax ≤ Zmin).  
    **Fix:** Verify that each `*max` value is strictly greater than the
    corresponding `*min` value. The defaults (0 → 10) are safe starting points.

!!! warning "delta x2 or z2 of wedge is negative — execution error"
    **Cause:** `X2max < X2min` or `Z2max < Z2min`. The top-face size can be zero
    (collapsing to a ridge) but cannot be negative.  
    **Fix:** Ensure `X2max ≥ X2min` and `Z2max ≥ Z2min`.

!!! warning "Wedge cavity does not appear at the expected location"
    **Cause:** All coordinates are measured in the Body's local coordinate system.
    If the Body origin is not where expected, the wedge appears in the wrong place.  
    **Fix:** Use the Attachment tool to reposition the wedge's local coordinate
    system, or adjust the ten coordinate values to account for the offset from the
    Body origin.

!!! warning "Inverted taper causes Boolean failure"
    **Cause:** When the top face is much larger than the base face in both
    dimensions, the wedge walls flare outward and may extend outside the base
    solid, leaving material uncut or causing unexpected geometry.  
    **Fix:** Ensure the entire wedge volume is contained within the base solid, or
    switch to a Pocket with a lofted sketch for more control over an inverted
    taper.

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
box.Width = 15.0
box.Height = 20.0
doc.recompute()

# Cut a tapered wedge slot from the top
wedge = doc.addObject("PartDesign::SubtractiveWedge", "Wedge")
body.addObject(wedge)

# Defaults: Xmin=0, Ymin=0, Zmin=0, X2min=2, Z2min=2,
#           Xmax=10, Ymax=10, Zmax=10, X2max=8, Z2max=8
doc.recompute()

print(f"Volume: {wedge.Shape.Volume:.4f} mm³")
# TODO: verify computed volume
```

### Customising the wedge — a ridge-top taper

```python
# Cavity with a full-width base and a ridge at the top (Z-ridge)
wedge.Xmin  = 0.0
wedge.Ymin  = 0.0
wedge.Zmin  = 0.0
wedge.Xmax  = 10.0
wedge.Ymax  = 8.0    # height of the cavity
wedge.Zmax  = 10.0
wedge.X2min = 0.0    # top face same width as base
wedge.X2max = 10.0
wedge.Z2min = 5.0    # top face collapses to a ridge at Z=5
wedge.Z2max = 5.0
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Xmin` | `App::PropertyDistance` | Minimum X of the base face, mm. |
| `Ymin` | `App::PropertyDistance` | Y coordinate of the base face, mm. |
| `Zmin` | `App::PropertyDistance` | Minimum Z of the base face, mm. |
| `X2min` | `App::PropertyDistance` | Minimum X of the top face, mm. |
| `Z2min` | `App::PropertyDistance` | Minimum Z of the top face, mm. |
| `Xmax` | `App::PropertyDistance` | Maximum X of the base face, mm. |
| `Ymax` | `App::PropertyDistance` | Y coordinate of the top face (wedge height), mm. |
| `Zmax` | `App::PropertyDistance` | Maximum Z of the base face, mm. |
| `X2max` | `App::PropertyDistance` | Maximum X of the top face, mm. |
| `Z2max` | `App::PropertyDistance` | Maximum Z of the top face, mm. |

---

## See also

- [Additive Wedge](additive-wedge.md) — same parameters, adds a wedge-shaped boss instead of cutting a cavity
- [Subtractive Box](subtractive-box.md) — simpler rectangular slot with Length/Width/Height parameters
- [Subtractive Prism](subtractive-prism.md) — regular polygon bore; simpler for symmetric shapes
- [Pocket](pocket.md) — remove material by extruding any closed sketch; use for non-standard tapers and positioned features
- [Subtractive Cone](subtractive-cone.md) — conical bore with a circular cross-section taper
