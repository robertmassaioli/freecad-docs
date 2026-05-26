# Pad

> **In one sentence:** Extrude a closed 2-D sketch into a 3-D solid by pushing it
> along its normal direction.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Additive Features → Pad  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Pad is the most fundamental additive feature in Part Design. It takes a closed
sketch (or a face selected from an existing solid) and extrudes it in a straight
line, fusing the resulting solid into the active Body. The extrusion distance can
be a fixed length, symmetric about the sketch plane, or driven by existing
geometry in the model — giving precise control over how far and in which direction
the material grows.

---

## Intuition

Think of a cookie cutter pressed into dough: the cutter's outline (the sketch)
determines the shape of the result, and how far you push it down (the length)
determines the thickness. You get a solid with the sketch's profile on the bottom
face, the same profile on the top face, and vertical walls connecting them.

The sketch plane is the starting face of the solid. The default direction is
perpendicular to that plane — the sketch's *normal*. If the sketch lies flat on
the XY plane, the pad grows in the Z direction. You can override this with a
custom direction or a reference axis if you need an oblique extrusion.

One important concept: Pad always *adds* material. If the sketch is larger than
the existing solid, the result is unioned with it. If you need to remove material
with the same straight-extrusion logic, use [Pocket](pocket.md) instead.

---

## When to use it

- You have drawn the outline of a part (a bracket, plate, or boss) and want it to
  have physical thickness.
- You are building a model parametrically and want the height of a feature to be
  driven by a named dimension.
- You need a solid to grow symmetrically on both sides of a sketch plane (use the
  **Symmetric** mode).
- You want a pad to stop exactly at an existing face in the model, regardless of
  how that face moves parametrically (use the **Up to face** or **Up to shape**
  mode).
- You need draft angles on the walls of a moulded or cast feature (use the taper
  angle option).

---

## When NOT to use it

- **The cross-section rotates around an axis** — use
  [Revolution](revolution.md); it is the right tool for axisymmetric solids such
  as shafts, flanges, and rings.
- **The cross-section needs to follow a curved path** — use
  [Additive Pipe](additive-pipe.md), which sweeps a profile along an arbitrary
  spine.
- **The solid should blend through several profiles in 3-D space** — use
  [Additive Loft](additive-loft.md).
- **You want to cut material** — use [Pocket](pocket.md), which has identical
  parameters but removes material from the Body instead of adding it.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one sketch with a single
closed wire. The sketch should be fully constrained.

1. **Select the sketch** in the model tree or by clicking it in the viewport.
   The sketch must be closed — an open wire produces an error.

2. **Activate Pad** via the menu **Part Design → Additive Features → Pad**, or
   click the Pad button in the toolbar.

3. **Set the Mode** (top of the task panel) to one of:
   - **One side** — extrude in one direction only (default).
   - **Two sides** — configure side 1 and side 2 independently.
   - **Symmetric** — extrude equal distances in both directions from the sketch
     plane.

4. **Set the Type** for Side 1 (and Side 2 if applicable). See the
   [Parameters](#parameters) table for all options. The most common choice is
   **Dimension**, where you type in an explicit length.

5. **Enter the length** in the **Length** field (visible when Type is
   *Dimension*). The default is 10 mm.

6. **Adjust optional settings** if needed:
   - **Taper angle** — adds draft to the extruded walls.
   - **Reversed** — flips the extrusion direction.
   - **Direction/edge** — change from the sketch normal to a custom axis.

7. **Click OK.** The pad is created and fused into the Body. The sketch becomes
   a child of the Pad feature in the model tree.

!!! tip
    If you activate Pad without pre-selecting a sketch, FreeCAD will ask you to
    select a profile. You can click a sketch in the model tree or click a planar
    face directly in the viewport — a face selection pads the entire face outline.

---

## Parameters

The task panel is divided into a *Mode* selector, sections for **Side 1** and
(optionally) **Side 2**, a **Reversed** toggle, and a **Direction** group.

### Mode

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Mode | Dropdown | One side | Controls how the two extrusion sides are configured. `One side`: only Side 1 is active. `Two sides`: Side 1 and Side 2 can be configured independently with different types and lengths. `Symmetric`: Side 1 settings apply equally in both directions; the total length is the Side 1 length. |

### Side 1 (and Side 2 when Mode = Two sides)

These controls appear for each active side.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Type | Dropdown | Dimension | Extrusion method. See the type descriptions below. |
| Length | Length | 10 mm | Extrusion length. Active only when Type is *Dimension*. Accepts negative values (reverses direction). |
| Offset to face | Length | 0 mm | Signed offset applied when the pad ends at a face or shape (types *To last*, *To first*, *Up to face*, *Up to shape*). Positive moves the end face away from the sketch; negative moves it toward the sketch. |
| Taper angle | Angle | 0° | Draft angle applied to the extruded walls. A positive angle expands the cross-section as it grows away from the sketch; a negative angle contracts it. Valid range is just under ±90°. Active only when Type is *Dimension*. |
| Select Face / Select Shape | Button | — | Opens selection mode to pick the target face or shape for *Up to face* / *Up to shape* types. |

**Type values:**

| Type (UI label) | Internal value | Description |
|-----------------|---------------|-------------|
| **Dimension** | `Length` | Extrude by a fixed distance set in the Length field. |
| **To last** | `UpToLast` | Extrude until the last face of the Body that the extrusion axis intersects. The pad grows to fill all remaining material in that direction. |
| **To first** | `UpToFirst` | Extrude until the nearest face of the Body that the extrusion axis intersects. |
| **Up to face** | `UpToFace` | Extrude until a specific face selected from the model. The pad stops at that face, following its shape even if it is curved. |
| **Up to shape** | `UpToShape` | Extrude until one or more faces of a selected shape or solid. Allows selecting multiple faces as the termination boundary. |

### Reversed and Direction

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reversed | Checkbox | Off | Flips the extrusion direction. For *One side* mode this pushes the pad to the opposite side of the sketch plane. Has no effect in *Symmetric* mode. |
| Direction/edge | Dropdown | Sketch normal | Sets the extrusion axis. `Sketch normal`: uses the sketch plane's perpendicular. `Select reference…`: pick an edge or axis from the model tree to use as the direction. `Custom direction`: activates the X/Y/Z vector fields. |
| Length along sketch normal | Checkbox | On | When using a custom direction, controls whether the Length value measures distance along the *sketch normal* or along the *custom direction vector*. Uncheck to measure along the custom vector instead. |
| X / Y / Z | Float | (0, 0, 1) | Components of the custom direction vector. Active only when Direction/edge is set to *Custom direction*. |

---

## Advanced usage

### Symmetric mode and taper

When **Mode** is *Symmetric* and a non-zero **Taper angle** is set, FreeCAD
creates two separate extrusion prisms — one in each direction — and fuses them.
Each prism tapers from the sketch plane outward, so the draft originates correctly
at the sketch and expands (or contracts) symmetrically on both sides.

Without a taper angle, the symmetric extrusion is built as a single prism
extending from −L/2 to +L/2 along the sketch normal.

### Up to shape vs. Up to face

**Up to face** targets a single named face. If the face moves parametrically
(e.g. it is the top face of another Pad whose length is changing), the Pad
automatically updates to track it.

**Up to shape** is more flexible: you select an entire solid or choose several
individual faces. The pad stops at the nearest point on any of the selected
faces. This is useful when the termination boundary is an irregular or
multi-face surface that cannot be represented as a single flat face.

### Custom direction for oblique pads

Setting a custom direction allows pads that are not perpendicular to their sketch
plane. This is useful for angled ribs or wedge features. Set **Direction/edge**
to *Custom direction*, enter a direction vector, and decide whether the **Length**
measures along the sketch normal (perpendicular height) or along the actual
extrusion vector. For most machined parts, measuring along the sketch normal
gives the most intuitive parametric behaviour.

### Combining Two sides with different types

In **Two sides** mode, Side 1 and Side 2 can have completely different types —
for example, Side 1 as *Dimension* (a fixed depth for a boss) and Side 2 as *To
last* (filling up to the opposite wall). This avoids needing a separate Pocket to
trim the other direction.

---

## Common mistakes and pitfalls

!!! warning "Sketch is not closed"
    **Cause:** The profile sketch has an open wire — a gap between two endpoints
    or an open arc — so FreeCAD cannot form a solid face.  
    **Fix:** Open the sketch, close every open wire (add a line or arc to fill
    the gap), and confirm that the Sketcher status bar reports zero open edges
    before closing.

!!! warning "Pad with zero total length"
    **Cause:** In *Dimension* mode, the sum of Side 1 and Side 2 lengths is zero
    (for example, `+5 mm` on Side 1 and `−5 mm` on Side 2).  
    **Fix:** Ensure at least one side has a non-zero length, or change the mode.

!!! warning "Direction is orthogonal to the sketch normal"
    **Cause:** A custom direction vector was entered that lies flat in the sketch
    plane. The extrusion cannot have any height in the normal direction.  
    **Fix:** Choose a direction vector that has a non-zero component along the
    sketch's normal axis.

!!! warning "Up to face / Up to last produces unexpected result"
    **Cause:** The target face is on the wrong side of the sketch plane, or the
    extrusion axis does not intersect any face in the correct direction.  
    **Fix:** Toggle **Reversed** to flip the extrusion direction so it points
    toward the intended termination face.

!!! warning "Taper angle causes invalid geometry"
    **Cause:** The taper angle is large enough that the extruded walls converge
    before reaching the specified length, creating a zero-area tip or a
    self-intersecting solid.  
    **Fix:** Reduce the taper angle, shorten the extrusion length, or scale up
    the profile.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher

doc = App.newDocument()

# Create a Body
body = doc.addObject("PartDesign::Body", "Body")

# Create a sketch on the XY plane
sketch = doc.addObject("Sketcher::SketchObject", "Sketch")
body.addObject(sketch)
sketch.AttachmentSupport = (doc.XY_Plane, [""])
sketch.MapMode = "FlatFace"
doc.recompute()

# Draw a 20×10 mm rectangle centred at the origin
sketch.addGeometry(Part.LineSegment(App.Vector(-10, -5, 0), App.Vector(10, -5, 0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(10, -5, 0), App.Vector(10, 5, 0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(10, 5, 0), App.Vector(-10, 5, 0)), False)
sketch.addGeometry(Part.LineSegment(App.Vector(-10, 5, 0), App.Vector(-10, -5, 0)), False)
# Add coincident constraints to close the rectangle  # TODO: verify full constraint set
sketch.addConstraint(Sketcher.Constraint("Coincident", 0, 2, 1, 1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 1, 2, 2, 1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 2, 2, 3, 1))
sketch.addConstraint(Sketcher.Constraint("Coincident", 3, 2, 0, 1))
doc.recompute()

# Create the Pad
pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = sketch
pad.Length = 15.0          # 15 mm extrusion
doc.recompute()

print(f"Volume: {pad.Shape.Volume:.1f} mm³")
# Expected: 20 × 10 × 15 = 3000.0 mm³
```

### Key properties

Properties are organised into property groups. `Side1` and `Side2` groups hold
per-direction settings; `Pad` holds shared settings.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The sketch or face used as the extrusion profile. Inherited from `ProfileBased`. |
| `SideType` | `App::PropertyEnumeration` | `"One side"` | Overall mode. Values: `"One side"`, `"Two sides"`, `"Symmetric"`. |
| `Type` | `App::PropertyEnumeration` | `"Length"` | Extrusion type for Side 1. Values: `"Length"`, `"UpToLast"`, `"UpToFirst"`, `"UpToFace"`, `"UpToShape"`. |
| `Type2` | `App::PropertyEnumeration` | `"Length"` | Extrusion type for Side 2 (used when `SideType` is `"Two sides"`). Same values as `Type`. |
| `Length` | `App::PropertyLength` | `10.0` | Extrusion length for Side 1 in mm. Accepts negative values. |
| `Length2` | `App::PropertyLength` | `10.0` | Extrusion length for Side 2 in mm. |
| `TaperAngle` | `App::PropertyAngle` | `0.0` | Draft angle for Side 1 in degrees. Range: just under ±90°. |
| `TaperAngle2` | `App::PropertyAngle` | `0.0` | Draft angle for Side 2 in degrees. |
| `UpToFace` | `App::PropertyLinkSub` | — | Target face for Side 1 when `Type` is `"UpToFace"`. |
| `UpToFace2` | `App::PropertyLinkSub` | — | Target face for Side 2 when `Type2` is `"UpToFace"`. |
| `UpToShape` | `App::PropertyLinkSubList` | — | Target shape/faces for Side 1 when `Type` is `"UpToShape"`. |
| `UpToShape2` | `App::PropertyLinkSubList` | — | Target shape/faces for Side 2 when `Type2` is `"UpToShape"`. |
| `Offset` | `App::PropertyLength` | `0.0` | Signed offset from the termination face for Side 1. |
| `Offset2` | `App::PropertyLength` | `0.0` | Signed offset from the termination face for Side 2. |
| `Reversed` | `App::PropertyBool` | `False` | When `True`, reverses the extrusion direction. |
| `UseCustomVector` | `App::PropertyBool` | `False` | When `True`, uses the `Direction` vector instead of the sketch normal. |
| `Direction` | `App::PropertyVector` | `(1, 1, 1)` | Custom extrusion direction vector. Normalised internally. Active only when `UseCustomVector` is `True`. |
| `ReferenceAxis` | `App::PropertyLinkSub` | — | Reference edge or axis to derive the extrusion direction from. |
| `AlongSketchNormal` | `App::PropertyBool` | `True` | When `True` and a custom direction is set, `Length` is measured along the sketch normal rather than along the custom vector. |
| `Midplane` | `App::PropertyBool` | `False` | **Deprecated since FreeCAD 1.1.** Use `SideType = "Symmetric"` instead. Setting this property emits a deprecation warning. |

!!! note "TwoLengths type is deprecated"
    The `"?TwoLengths"` value is present in the internal enum for file
    compatibility only (it was replaced by `SideType = "Two sides"` in
    FreeCAD 1.1 — see PR #21794). Do not set `Type` to `"TwoLengths"` in new
    scripts; use `SideType = "Two sides"` with separate `Length` and `Length2`
    values.

---

## See also

- [Pocket](pocket.md) — identical parameters, removes material instead of adding it
- [Revolution](revolution.md) — rotates a profile around an axis; the right choice for axisymmetric parts
- [Additive Pipe](additive-pipe.md) — sweeps a profile along a curved spine
- [Additive Loft](additive-loft.md) — blends through multiple profiles in 3-D space
- [Additive Helix](additive-helix.md) — sweeps a profile along a helix for springs and threads
