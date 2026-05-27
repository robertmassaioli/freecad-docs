# Pocket

> **In one sentence:** Cut a closed 2-D sketch into an existing solid by pushing
> it along its normal direction, removing material from the Body.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Subtractive Features → Pocket  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Pocket is the subtractive counterpart to [Pad](pad.md). It takes a closed sketch
and drives it into the Body as a Boolean cut, removing material along a straight
path. The cut depth can be a fixed length, symmetric about the sketch plane,
driven by the geometry of existing features, or through the entire remaining solid.
All of Pad's extrusion modes and direction controls are available in Pocket.

---

## Intuition

Think of a rubber stamp pressed into clay: the stamp's outline (the sketch) and
the force you apply (the depth) determine the shape and depth of the impression.
The clay around the stamp is untouched; only material inside the sketch boundary
is removed.

The sketch plane sits at the top of the cut. The default cutting direction is
*into* the solid along the sketch's normal — the mirror image of how Pad works.
If your sketch lies on the XY plane of the Body, the cut proceeds in the −Z
direction (into the solid below the sketch).

Because Pocket is parametric, you can change the depth at any time and the model
updates automatically. The feature stores the sketch as a reference, so modifying
the sketch shape also updates the cut.

---

## When to use it

- You need a blind hole, slot, channel, or any recess that goes partway through an
  existing solid.
- You want to cut completely through a solid without specifying an exact depth —
  use **Through all** to guarantee the cut always exits the other side regardless
  of how the model changes.
- You are cutting a feature whose depth should be driven by the geometry of
  another face in the model (use **Up to face** or **Up to shape**).
- You need the cut to be symmetric about the sketch plane — for example, a slot
  centred on a parting line (use the **Symmetric** side mode).
- You need independent depths on both sides of the sketch plane (use the
  **Two sides** side mode with a separate length for each direction).
- You want walls of the cut to taper outward or inward (use the taper angle
  option).

---

## When NOT to use it

- **You want to add material, not remove it** — use [Pad](pad.md) instead.
- **The cross-section rotates around an axis** — use [Groove](groove.md), the
  subtractive counterpart to Revolution.
- **The cut should follow a curved path** — use
  [Subtractive Pipe](subtractive-pipe.md).
- **The cut should blend through several profiles** — use
  [Subtractive Loft](subtractive-loft.md).
- **You need a standard drilled or tapped hole with a countersink or counterbore**
  — use [Hole](hole.md), which handles thread geometry automatically.

---

## Step-by-step walkthrough

**Prerequisite:** An active Body containing at least one existing solid (from a
previous Pad, Revolution, or other additive feature) and a sketch drawn on one of
its faces or on a datum plane. The sketch must be a single closed wire and should
be fully constrained.

1. **Select the sketch** in the model tree or by clicking it in the viewport.

2. **Activate Pocket** via **Part Design → Subtractive Features → Pocket**, or
   click the Pocket button in the **Part Design Modeling Features** toolbar.

3. **Choose the Side Mode** from the *Mode* dropdown at the top of the task
   panel. `One side` is the default. Select `Two sides` for independent depths in
   both directions, or `Symmetric` to split the depth equally on each side.

4. **Set the Type for Side 1.** Choose one of the depth modes from the *Type*
   dropdown (see [Parameters](#parameters) for all options). `Dimension` is the
   default.

5. **Enter the length** in the *Length* field (active when Type is `Dimension`).
   The default is 5 mm. Enter any value; negative values reverse the direction.

6. **Optionally set a taper angle.** A positive taper widens the cut toward the
   sketch plane (draft for moulding); a negative taper narrows it.

7. **If using Two sides,** repeat the Type and Length settings for Side 2 in the
   second section of the task panel.

8. **Set direction.** In the *Direction* group you can override the sketch normal
   with a reference edge or a custom vector. Leave it on `Sketch normal` for most
   cuts.

9. **Check Reversed** if the cut is going in the wrong direction (for example, if
   the solid is below the sketch but the cut is trying to go upward).

10. **Click OK.** The solid is updated with the cut removed. The Pocket feature
    appears in the model tree.

!!! tip
    For a cut that must always go completely through the part regardless of its
    thickness, use **Through all** rather than guessing a large length value.
    Through all is robust to model changes; a hard-coded depth is not.

!!! warning "No base solid exists yet"
    If Pocket is the first feature in the Body (no prior Pad or other additive
    feature), the operation produces an error. Pocket requires existing material
    to cut into. Add a Pad first to create the base solid, then apply Pocket.

---

## Parameters

The task panel is divided into a *Mode* selector, a *Side 1* group, an optional
*Side 2* group (visible when Mode is **Two sides**), a *Direction* group, and
a *Reversed* checkbox.

### Mode

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Mode | Dropdown | `One side` | Controls how the two directions of the pocket are treated. `One side`: single depth in one direction. `Two sides`: independent Type and Length for each direction. `Symmetric`: one Length applied equally on both sides of the sketch plane. |

### Mode in depth

Mode is the first decision to make — it determines how many active "sides" the
pocket has before you configure any depths.

#### One side (default)

The pocket cuts from the sketch plane in a single direction only. The vast
majority of pockets use One side.

**Use this mode when:**
- Cutting a standard slot, blind hole, or recess in one direction
- Only Side 1 depth and Type settings are needed

**Watch out for:** Nothing unusual — One side is correct for almost all pockets.
Switch to Two sides or Symmetric only when you genuinely need both directions.

#### Two sides

Each side of the sketch plane gets its own independent Type and Length. The
pocket can cut deeper on one side than the other, or use different stop modes
on each side.

**Use this mode when:**
- The pocket must extend different distances in each direction from the sketch
  (e.g. a slot centred on a mid-plane feature but asymmetric)
- One side is Through all while the other has a fixed dimension
- Avoiding two separate Pocket features by capturing both directions in one

**Watch out for:** FreeCAD defines Side 1 as the direction of the sketch normal.
Side 2 is the opposite direction. Verify which is which using the live preview
before committing.

#### Symmetric

A single Length value is applied equally on both sides of the sketch plane.
Equivalent to Two sides with both lengths set to L/2 and both Types set to
Dimension — but entered as a single value.

**Use this mode when:**
- The pocket must be exactly centred on a mid-plane or parting line
- Avoiding the arithmetic of calculating half-depths manually

**Watch out for:** Symmetric locks both sides to Dimension type. If you need one
side to stop at a face or cut Through all, switch to Two sides instead.

### Side 1 (and Side 2 when Mode is Two sides)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Type | Dropdown | `Dimension` | Depth mode for this side. See type descriptions below. |
| Length | Length | `5 mm` | Depth of cut when Type is `Dimension`. Accepts negative values, which reverse the cut direction. |
| Offset to face | Length | `0 mm` | A signed offset from the stop face when Type is `Up to face` or `Up to shape`. Positive pushes the cut floor further past the face; negative stops it short. |
| Taper angle | Angle | `0°` | Draft angle applied to the cut walls. Positive widens toward the sketch; negative narrows. Valid range: any angle that does not cause self-intersection. |
| Select Face / Select Shape | Button + text field | — | Active when Type is `Up to face` or `Up to shape`. Click to enter selection mode and pick the target face or shape in the viewport. |

**Type options:**

| Type | UI label | Behaviour |
|------|----------|-----------|
| `Length` | Dimension | Cut a fixed depth given by the Length field. |
| `ThroughAll` | Through all | Cut through the entire remaining solid. No Length input required. |
| `UpToFirst` | To first | Stop at the first face the cut encounters along its direction. |
| `UpToFace` | Up to face | Stop exactly at a selected face; floor follows that face's shape. |
| `UpToShape` | Up to shape | Stop at one or more selected faces or an entire shape. |

### Type in depth

The Type dropdown determines *when* the cut stops on each side.

#### Dimension (default)

You specify the exact cut depth numerically. Think of drilling to a precise depth
marked on a ruler.

**Use this mode when:**
- The depth is a fixed design requirement
- You want a taper angle on the cut walls (only available with Dimension)
- The cut depth is an independent parameter, not derived from other features

**Watch out for:** A Dimension pocket deeper than the solid finds no material to
remove and fails. The solid must be at least as thick as the requested depth in
the cut direction.

#### Through all

The cut goes through all remaining material in the extrusion direction —
regardless of how thick the solid is. If the solid grows or shrinks, the cut
adapts automatically.

**Use this mode when:**
- Cutting a hole or slot that must pass completely through the part
- The solid thickness is parametrically driven and you do not want to track it
- Making through-features that should always penetrate the full wall

**Watch out for:** "All" means the entire solid in that direction, following the
cut direction vector. If a custom direction is set at an angle, the cut still
goes all the way through — just at that angle.

#### To first

The cut stops at the first face it encounters in the cut direction.

**Use this mode when:**
- Cutting to an existing inner wall or shelf within the solid
- The stopping face is the nearest obstruction in the cut direction

**Watch out for:** If there is no face between the sketch and the solid's outer
boundary, To first behaves unpredictably. Always confirm which face is "first"
in the live preview.

#### Up to face

You select a specific face; the cut floor follows that face's shape exactly,
even if it is curved or angled.

**Use this mode when:**
- The pocket floor must conform to a curved or angled mating surface
- The floor depth must track a named face as the model changes parametrically

**Watch out for:** The selected face must be reachable — it must intersect the
cut volume along the cut direction. A face parallel to the cut direction is never
intersected and will cause a failure.

#### Up to shape

Like Up to face but accepts multiple individual faces or an entire solid as the
boundary. The floor of the cut follows the nearest point of any selected surface.

**Use this mode when:**
- The floor boundary spans more than one face
- Cutting to the outer hull of an imported STEP or solid

**Watch out for:** Selecting too many faces can produce ambiguous floor geometry.
Pick only the surfaces that form the intended floor boundary.

!!! note "Deprecated type"
    A sixth internal enum value `TwoLengths` exists in the file format for
    backwards compatibility with documents created before FreeCAD 1.0. It is no
    longer exposed in the UI; open such documents and FreeCAD will migrate the
    feature to the equivalent **Two sides** mode automatically.

### Direction group

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Direction/edge | Dropdown | `Sketch normal` | The axis along which the pocket is cut. `Sketch normal`: perpendicular to the sketch plane (default). `Select reference…`: pick an edge or axis from the model. `Custom direction`: enter X/Y/Z components manually. |
| Length along sketch normal | Checkbox | On | When checked, the Length value measures along the sketch normal regardless of the chosen direction. When unchecked, Length measures along the specified direction vector. |
| X / Y / Z | Float | 0 / 0 / 1 | Custom direction vector components; active only when Direction is set to `Custom direction`. |

### Global options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Reversed | Checkbox | Off | Reverses the cut direction. Use this when FreeCAD's auto-detect chooses the wrong side. |
| Recompute on change | Checkbox | On | When on, the model updates live as you adjust parameters in the task panel. Uncheck to defer recomputing for performance when working with complex models. |

---

## Advanced usage

### Cutting with a custom direction

By default Pocket cuts along the sketch's normal. Select `Custom direction` in the
*Direction/edge* dropdown and enter X/Y/Z components to cut at any angle. This is
useful when, for example, cutting on a draft angle from the start rather than
applying a taper, or when the sketch plane is not aligned with the desired cut
direction.

When using a custom direction, the `Length along sketch normal` checkbox changes
meaning: if checked, the length still measures normal to the sketch face (so the
cut depth as seen from above stays constant); if unchecked, the length measures
along the custom direction vector.

### Two-sided pockets

`Two sides` mode lets you specify different Type and Length values for each side of
the sketch plane independently. This is useful for slots or grooves that extend
different distances in each direction, or when one side should be `Through all`
while the other has a fixed depth.

### Symmetric pockets

`Symmetric` mode uses a single Length value and applies half of it on each side of
the sketch plane. This is convenient when the pocket must be centred on a parting
line or mid-plane without manually calculating half-depths.

### Up to shape

`Up to shape` extends `Up to face` to allow multiple faces or a full imported
solid as the stop surface. This is useful when the cut floor follows a complex or
non-planar target that cannot be described by a single face.

---

## Common mistakes and pitfalls

!!! warning "Sketch is not closed"
    **Cause:** The sketch has one or more open wires — a gap between endpoints
    that prevents FreeCAD from forming a solid face.  
    **Fix:** Open the sketch, find and close the gap (add a closing segment or use
    Sketcher's *Close shape* tool), and verify that the Sketcher reports zero open
    edges before closing.

!!! warning "Pocket goes in the wrong direction"
    **Cause:** FreeCAD's heuristic for choosing the cut direction picked the side
    opposite to the solid.  
    **Fix:** Check the **Reversed** checkbox in the task panel to flip the cut
    direction.

!!! warning "No base solid — Pocket is the first feature"
    **Cause:** Pocket requires existing material to cut into. If it is the first
    feature in the Body, there is nothing to remove.  
    **Fix:** Add a Pad or other additive feature before the Pocket to create the
    base solid.

!!! warning "Cut does not reach the target face (Up to face)"
    **Cause:** The selected face is not intersected by the cut along its direction,
    or the sketch is positioned outside the target face's extent.  
    **Fix:** Verify that the cut direction (sketch normal or custom vector) actually
    points toward the selected face. Consider using **Up to shape** with the whole
    object if the target face has complex geometry.

!!! warning "Sketch axis intersects sketch profile (axis collision)"
    **Cause:** Not applicable to Pocket directly, but if you accidentally use
    Groove when you intended Pocket, OCCT will error because the revolve axis
    passes through the sketch.  
    **Fix:** Confirm you are using Pocket (straight extrusion), not Groove.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part
import Sketcher
import PartDesign  # noqa: F401 — registers PartDesign types

doc = App.newDocument()

# Create a body with a base pad
body = doc.addObject("PartDesign::Body", "Body")

# Base sketch: 40 × 30 mm rectangle on the XY plane
base_sk = doc.addObject("Sketcher::SketchObject", "BaseSketch")
body.addObject(base_sk)
base_sk.addGeometry(Part.LineSegment(App.Vector(0,  0, 0), App.Vector(40,  0, 0)), False)
base_sk.addGeometry(Part.LineSegment(App.Vector(40, 0, 0), App.Vector(40, 30, 0)), False)
base_sk.addGeometry(Part.LineSegment(App.Vector(40,30, 0), App.Vector( 0, 30, 0)), False)
base_sk.addGeometry(Part.LineSegment(App.Vector( 0,30, 0), App.Vector( 0,  0, 0)), False)
# TODO: verify — add coincident constraints to fully close the sketch
doc.recompute()

pad = doc.addObject("PartDesign::Pad", "Pad")
body.addObject(pad)
pad.Profile = base_sk
pad.Length = 20.0
doc.recompute()

# Pocket sketch: 10 × 10 mm square on the top face
pocket_sk = doc.addObject("Sketcher::SketchObject", "PocketSketch")
body.addObject(pocket_sk)
pocket_sk.MapMode = "FlatFace"
pocket_sk.AttachmentSupport = (pad, ["Face6"])  # top face; index may vary
doc.recompute()
pocket_sk.addGeometry(Part.LineSegment(App.Vector(15, 10, 20), App.Vector(25, 10, 20)), False)
pocket_sk.addGeometry(Part.LineSegment(App.Vector(25, 10, 20), App.Vector(25, 20, 20)), False)
pocket_sk.addGeometry(Part.LineSegment(App.Vector(25, 20, 20), App.Vector(15, 20, 20)), False)
pocket_sk.addGeometry(Part.LineSegment(App.Vector(15, 20, 20), App.Vector(15, 10, 20)), False)
doc.recompute()

# Create the pocket: 8 mm deep
pocket = doc.addObject("PartDesign::Pocket", "Pocket")
body.addObject(pocket)
pocket.Profile = pocket_sk
pocket.Type = "Length"    # "Length" is the internal enum value for "Dimension"
pocket.Length = 8.0
doc.recompute()

print(f"Volume after pocket: {pocket.Shape.Volume:.2f} mm³")
# Expected: 40×30×20 − 10×10×8 = 24000 − 800 = 23200 mm³  # TODO: verify
```

### Key properties

Pocket inherits all properties from `FeatureExtrude` and `ProfileBased`. The most
important ones are listed below.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Profile` | `App::PropertyLinkSub` | — | The sketch (or face sub-element) defining the cut profile. Inherited from `ProfileBased`. |
| `SideType` | `App::PropertyEnumeration` | `"One side"` | Top-level mode. Values: `"One side"`, `"Two sides"`, `"Symmetric"`. |
| `Type` | `App::PropertyEnumeration` | `"Length"` | Depth mode for Side 1. Values: `"Length"`, `"ThroughAll"`, `"UpToFirst"`, `"UpToFace"`, `"UpToShape"`. |
| `Type2` | `App::PropertyEnumeration` | `"Length"` | Depth mode for Side 2. Same values as `Type`. Active only when `SideType` is `"Two sides"`. |
| `Length` | `App::PropertyLength` | `5 mm` | Depth for Side 1 when `Type` is `"Length"`. Accepts negative values. |
| `Length2` | `App::PropertyLength` | `5 mm` | Depth for Side 2. Active only when `SideType` is `"Two sides"` and `Type2` is `"Length"`. |
| `TaperAngle` | `App::PropertyAngle` | `0°` | Draft angle for Side 1 walls. |
| `TaperAngle2` | `App::PropertyAngle` | `0°` | Draft angle for Side 2 walls. |
| `Offset` | `App::PropertyLength` | `0 mm` | Signed offset from the target face for `UpToFace`/`UpToShape` on Side 1. |
| `Offset2` | `App::PropertyLength` | `0 mm` | Signed offset for Side 2. |
| `UpToFace` | `App::PropertyLinkSub` | — | The target face when `Type` is `"UpToFace"`. |
| `UpToShape` | `App::PropertyLinkSubList` | — | Target faces or shape when `Type` is `"UpToShape"`. |
| `UseCustomVector` | `App::PropertyBool` | `False` | When `True`, the `Direction` vector overrides the sketch normal. |
| `Direction` | `App::PropertyVector` | `(1, 1, 1)` | Custom cut direction when `UseCustomVector` is `True`. Will be normalised. |
| `ReferenceAxis` | `App::PropertyLinkSub` | — | Reference edge or axis used as the cut direction instead of the sketch normal. |
| `AlongSketchNormal` | `App::PropertyBool` | `True` | When `True`, the `Length` value measures along the sketch normal even if a custom direction is set. |
| `Reversed` | `App::PropertyBool` | `False` | Reverses the cut direction. Inherited from `ProfileBased`. |
| `Midplane` | `App::PropertyBool` | `False` | **Deprecated.** Setting this to `True` is equivalent to setting `SideType` to `"Symmetric"`. Use `SideType` in new scripts. |

---

## See also

- [Pad](pad.md) — identical parameters, adds material instead of removing it
- [Groove](groove.md) — subtractive revolution; the rotational equivalent of Pocket
- [Hole](hole.md) — specialised pocket for drilled/tapped holes with thread geometry
- [Subtractive Pipe](subtractive-pipe.md) — cuts material along a swept path
- [Subtractive Loft](subtractive-loft.md) — cuts material by blending through profiles
- [Revolution](revolution.md) — additive counterpart to Groove
