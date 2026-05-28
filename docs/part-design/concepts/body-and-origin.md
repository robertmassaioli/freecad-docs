# Body, Origin, and the Feature Tree

> **In one sentence:** The Part Design Body is the container that holds all
> features of a single solid; the Origin provides its built-in reference
> planes and axes; and the Tip marks where in the feature sequence FreeCAD
> stops evaluating — making it the "current shape" of the Body.

---

## The Body

A **Body** (`PartDesign::Body`) is the fundamental container for one solid
part in the Part Design workflow. Every Pad, Pocket, Fillet, and other
feature must live inside a Body. You cannot have free-floating Part Design
features outside a Body.

### What a Body contains

When you create a new Body, FreeCAD automatically places the following
objects inside it:

| Object | Type | Purpose |
|--------|------|---------|
| **Origin** | `App::Origin` | Built-in reference geometry: three planes (XY, XZ, YZ) and three axes (X, Y, Z) |
| *(your features)* | Various `PartDesign::*` | Pad, Pocket, Fillet, etc., added by you |

Each feature is ordered in the Body's feature tree. The Body evaluates
the features **from top to bottom**, chaining the shape of each feature
into the next. Reordering features (with [Move Feature in Tree](../tools/move-feature-in-tree.md))
changes the final shape.

### One Body = one solid

A Body represents one contiguous solid body — one connected piece of material.
A FreeCAD document can contain multiple Bodies (for multi-part assemblies),
but each Body is independent. To combine two Bodies into a single solid, use
[Boolean](../tools/boolean.md).

### Active Body

Only one Body is **active** at a time. New features are automatically placed
into the active Body. To activate a Body, double-click it in the model tree.
The active Body is highlighted in the tree and its features appear in the
3-D view.

---

## The Origin

Every Body automatically contains an **Origin** object. The Origin provides
built-in reference geometry anchored to the Body's local coordinate system:

### Origin contents

| Reference | Type | Axis / Normal |
|-----------|------|--------------|
| **XY Plane** | Reference plane | Normal along Z |
| **XZ Plane** | Reference plane | Normal along Y |
| **YZ Plane** | Reference plane | Normal along X |
| **X Axis** | Reference axis | Points in +X |
| **Y Axis** | Reference axis | Points in +Y |
| **Z Axis** | Reference axis | Points in +Z |

All six are always available. Show or hide the Origin in the 3-D view by
pressing **Space** while the Origin is selected, or toggle its visibility
from the Model tree.

### Using the Origin for sketches

The most common use of the Origin is as the plane for the very first
sketch. Before any external geometry exists, the Body's Origin planes are
the only reference available:

1. Select `XY_Plane` (or `XZ_Plane` or `YZ_Plane`) inside the Body's
   Origin.
2. Create a new sketch — it is mapped to that plane.
3. Draw and constrain the profile.
4. Apply a Pad or Revolution.

The Origin is always at the Body's local (0, 0, 0). Moving the Body's
`Placement` moves the entire solid — including the Origin — while keeping
all internal feature geometry intact.

### Origin vs Datum Planes

| | Origin plane | Datum Plane |
|--|-------------|-------------|
| Created automatically | Yes | No — you add them |
| Count | Three fixed (XY, XZ, YZ) | As many as you need |
| Position | Fixed at Body origin | Anywhere you attach them |
| Editable | No | Yes |
| Use | First sketch reference; axis references | Additional planes for later features |

Origin planes are sufficient for simple parts. For complex parts requiring
angled sketch planes or planes offset from the body centre, create
[Datum Planes](../tools/datum-plane.md).

---

## The Feature Tree

The feature tree is the ordered list of features inside the Body. It is
the core of Part Design's **parametric history** model.

### How the feature tree works

```
Body
├── Origin
│     ├── XY_Plane
│     ├── XZ_Plane
│     ├── YZ_Plane
│     ├── X_Axis
│     ├── Y_Axis
│     └── Z_Axis
├── Sketch           ← profile for the first Pad
├── Pad              ← first additive feature
├── Sketch001        ← profile for the Pocket
├── Pocket           ← subtractive feature
├── Fillet           ← dress-up
└── LinearPattern    ← transformation   (← Tip is here)
```

FreeCAD evaluates the tree **top to bottom**, computing a cumulative shape
at each step. The result at the bottom of the tree is the Body's current
shape — visible in the 3-D view.

### Why order matters

Because each feature adds to or modifies the shape produced by all
preceding features:

- A Fillet applied to an edge that is later removed by a Pocket
  will fail — the edge it referenced no longer exists.
- A LinearPattern below a Pocket repeats only the patterned feature(s),
  not the whole body from before the Pocket (unless Transform Body mode
  is used).
- Reordering features can fix dependency errors, allow new features to
  reference edges that didn't exist earlier, or change the final shape.

---

## The Tip

The **Tip** is a marker in the Body that indicates which feature is
currently the "end" of the evaluation sequence. FreeCAD computes the solid
only up to and including the Tip; features below the Tip in the tree are
not included in the body's displayed shape.

### Why Tip matters

- **Adding features:** When you add a new feature, it is inserted just
  below the current Tip. Moving the Tip lets you insert features at any
  point in the history — not just at the end.
- **Inspecting history:** Moving the Tip backwards lets you see what the
  part looked like at any earlier stage of the design.
- **Recovering from errors:** If a later feature fails, moving the Tip
  to the last valid feature lets you work on the part before re-fixing
  the failing feature.

### Moving the Tip

Use **[Move Tip](../tools/move-tip.md)** (right-click → Set Tip) on any
feature in the Body. The Tip jumps to that feature, and all features after
it are temporarily excluded from the shape evaluation.

The Tip is shown in the model tree with a small green marker on the
feature's icon.

### Tip and new features

When you add a new Pad, Pocket, or other feature, FreeCAD places it
immediately after the current Tip and advances the Tip to the new feature.
This is the expected behaviour: new features build on top of the current
shape.

If you need to insert a feature in the middle of the history:
1. Move the Tip to the feature just above where you want to insert.
2. Add the new feature (it goes after the current Tip).
3. Move the Tip back to the end (or to the final feature).

---

## Practical tips

!!! tip "Always start with a sketch on the XY plane"
    For most parts, the first sketch belongs on the Body's XY plane (Z-up
    convention). This ensures that **View → Standard Views → Top** shows
    the part plan view, and **Front** shows the elevation — matching
    engineering drawing conventions.

!!! tip "One part = one Body"
    Resist the temptation to put multiple independent parts in the same
    Body using Boolean. Use one Body per physical component and combine
    them in the Assembly workbench or with Part Design Boolean at the top
    level.

!!! warning "Multi-Body booleans require explicit Boolean feature"
    Two separate Bodies do not automatically interact. Even if they visually
    overlap in the 3-D view, each Body's solid is independent. Use
    [Boolean](../tools/boolean.md) to fuse, cut, or intersect two Bodies
    into a single solid.

!!! warning "Features after the Tip are invisible but not deleted"
    Moving the Tip hides features below it from the shape computation.
    Those features still exist and their parameters are preserved. Moving
    the Tip back restores them immediately.

---

## See also

- [Body tool](../tools/body.md) — creating and managing Body containers
- [Move Tip](../tools/move-tip.md) — setting the evaluation end point
- [Move Feature in Tree](../tools/move-feature-in-tree.md) — reordering features
- [Boolean](../tools/boolean.md) — combining two Bodies into one solid
- [Datum Plane](../tools/datum-plane.md) — adding custom reference planes
- [Attachment](attachment.md) — how datum objects and primitives are positioned
