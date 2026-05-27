# Body

> **In one sentence:** Create an independent solid container that holds all
> Part Design features and owns the Origin geometry they attach to.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → New Body  
**Toolbar:** Part Design Helper Features · **Shortcut:** none

A Body is the root container for a single solid in the Part Design workflow.
Every sketch, datum, and shaping feature (Pad, Pocket, Fillet, …) must live
inside a Body. When you create a Body, FreeCAD immediately activates it so that
subsequent operations are placed inside it, and automatically creates an Origin
— three reference planes (XY, XZ, YZ), three reference axes (X, Y, Z), and an
origin point — that everything else can attach to.

---

## Intuition

Think of a Body as an individual machined part sitting on a workbench. The
workbench itself has built-in reference lines and a flat top surface (the
Origin). Everything you make — sketches, extrusions, holes, fillets — goes on
or into that one part. When you are done, the part is a single solid, and that
is what you hand to someone.

The "feature tree" inside the Body records every operation you applied, in
order, like a machinist's job sheet. You can rewind to any step (by moving the
*Tip*), change a dimension, and FreeCAD re-applies every subsequent operation
automatically. The Body is what makes that history possible: it owns the
sequence and produces the final shape as the cumulative result.

A document can contain multiple Bodies — one for each independent solid in the
assembly. Bodies do not automatically interact with each other; joining them
requires a Boolean operation or a Part container.

---

## When to use it

- You are starting a new Part Design model and need a container for the first
  sketch and feature.
- You are designing a document that contains several independent solid
  components (e.g. a bolt and a nut in the same file) — create one Body per
  component.
- You want to import an existing Part solid (a box, a sphere, a body from
  another workbench) and continue adding Part Design features on top of it —
  select the solid before creating the Body to use it as a Base Feature.
- You need the clean Origin reference geometry (datum planes and axes) that a
  Body provides to anchor your first sketch.

---

## When NOT to use it

- **You already have an active Body and just want to add another feature to
  it** — do not create a new Body; instead use
  [New Sketch](new-sketch.md) or any additive/subtractive <!-- TODO: verify link -->
  feature directly. Creating a second Body produces a completely independent
  solid.
- **You want to combine two existing Bodies into one solid** — use
  [Boolean](boolean.md) to merge, cut, or intersect them; a Body alone does
  not perform that combination.
- **You are working in the Part workbench** — that workbench does not use
  Bodies. Bodies are exclusive to the Part Design workflow.

---

## Step-by-step walkthrough

**Prerequisites:** An open FreeCAD document. No other precondition is required;
a Body can be the very first object in a new document.

1. **Optionally select a base solid.** If you want the Body to start from an
   existing Part solid (e.g. a box from the Part workbench), click it in the
   model tree *before* activating the command. The solid becomes the Body's
   *Base Feature* — the first solid in its feature chain.

    !!! warning "Base Feature restrictions"
        The selected solid must not already belong to another Body and must not
        itself be a Body. Selecting multiple solids will prevent the command
        from running — only one base feature is permitted. If the selected
        feature has multiple solids or shells, a warning dialog appears.

2. **Activate the command** via one of:
    - Menu: **Part Design → New Body**
    - Toolbar: click the **Body** button in the *Part Design Helper Features*
      toolbar

3. **Observe what appears in the model tree.** FreeCAD:
    - Creates a `PartDesign::Body` object labelled **Body** (or **Body001**,
      etc. if names are already taken).
    - Creates an **Origin** object inside it containing the XY, XZ, and YZ
      reference planes, the X, Y, and Z axes, and the origin point.
    - If a base feature was selected, adds a **BaseFeature** proxy inside the
      Body that wraps the selected solid.
    - Makes the new Body the **active body** for the current view.

4. **Begin building the model.** With the Body active, create a sketch
   (Part Design → New Sketch or the toolbar button). The Sketcher opens and
   you select one of the Body's Origin planes as the attachment plane. Close
   the sketch, then apply an additive feature such as [Pad](pad.md).

5. **Keep adding features.** Each subsequent sketch and feature is
   automatically placed inside the active Body and appended to its feature
   tree. The *Tip* (the last solid feature) updates each time.

!!! tip
    If you accidentally create a Body when you meant to add to an existing one,
    undo the action (`Ctrl+Z`), then double-click the intended Body in the tree
    to re-activate it before continuing.

---

## Parameters

A Body has no task-panel dialog. Its parameters appear in the **Property
editor** (bottom-left panel in the default layout) when the Body is selected.

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `Label` | String | `Body` | Human-readable name shown in the model tree. Editable; auto-incremented to avoid duplicates. |
| `Tip` | Link | `None` | The feature that provides the Body's current shape. Defaults to the last solid feature added. Move the tip to "rewind" the history. |
| `BaseFeature` | Link | `None` | An external Part solid that acts as the starting shape for the first feature inside the Body. Set at creation time if a solid was selected; not normally changed later. |
| `Group` | LinkList | `[]` | The ordered list of all objects inside the Body (sketches, datums, solid features, binders). Managed automatically; edit with caution. |
| `AllowCompound` | Bool | `True` | When `True`, features that produce multiple disconnected solids are allowed (the Body's shape becomes a compound). When `False`, the first feature that would create multiple solids causes a recompute error. The default at creation time follows the preference `BaseApp/Preferences/Mod/PartDesign/AllowCompoundDefault`. |
| `Placement` | Placement | identity | Position and orientation of the entire Body in world space. Rarely needed — moving individual features is preferred. |
| `ShapeMaterial` | Material | — | Appearance material applied to the Body's shape and propagated to all contained features. |

---

## Advanced usage

### Multiple Bodies in one document

Each Body produces one independent solid. A document can contain as many Bodies
as needed. To work on a specific Body, double-click it in the model tree to
make it active; its label turns bold. Features created while a Body is active
are added to that Body.

To combine two Body solids (e.g. subtract one from the other), use
[Boolean](boolean.md). To reference geometry from one Body inside another,
use [Sub-Shape Binder](sub-shape-binder.md).

### Using an existing solid as a Base Feature

Any `Part::Feature`-derived solid (a box from the Part workbench, an imported
STEP shape, a FreeCAD Mesh converted to a solid) can serve as the starting
point for a Body. Select the solid before creating the Body; FreeCAD wraps it
in an internal `FeatureBase` proxy and presents it as the first step in the
feature chain. All subsequent Part Design features add to or subtract from that
base.

If the selected object is a Sketcher sketch rather than a solid, FreeCAD adds
the sketch to the Body's group and prompts you to choose which Origin plane to
attach it to.

### Moving the Tip to inspect intermediate states

Right-click any feature in the Body's model tree and choose **Set Tip** (or use
Part Design → Set Tip) to rewind the history to that feature. The viewport
shows the shape as it was at that point, and new features are inserted after
the Tip rather than at the end of the tree. Move the Tip back to the last
feature to resume normal editing.

### AllowCompound and multi-solid models

With `AllowCompound = True` (the default), operations that accidentally
disconnect the solid — such as a pattern that places copies apart from the main
body — produce a compound result without error. Set `AllowCompound = False` to
enforce single-solid discipline; FreeCAD will then report an error on the
feature that first creates multiple disconnected solids, making the problem
visible immediately.

---

## Common mistakes and pitfalls

!!! warning "Feature created outside the active Body"
    **Cause:** No Body was active when a sketch or feature was created, or the
    wrong Body was active. The feature appears as a top-level document object
    rather than inside the intended Body.  
    **Fix:** Use Part Design → Move Object To… to reassign the feature to the
    correct Body, or undo and re-activate the intended Body before recreating
    the feature.

!!! warning "Two Bodies instead of one solid"
    **Cause:** A second Body was created instead of continuing to add features
    to the first one. The model tree shows two separate Body objects.  
    **Fix:** Either delete the second Body and re-activate the first, or — if
    both Bodies contain useful geometry — combine them with
    [Boolean](boolean.md).

!!! warning "Tip not at the end of the tree"
    **Cause:** The Tip was moved to an intermediate feature (e.g. to inspect a
    step) and not moved back. Subsequent features are inserted after the current
    Tip, not at the end, which can produce unexpected geometry.  
    **Fix:** Right-click the last feature in the tree and choose **Set Tip**
    (or Part Design → Set Tip) to restore normal behaviour.

!!! warning "Base feature already belongs to another Body"
    **Cause:** The solid selected before running New Body is already inside a
    Body (or is itself a Body). FreeCAD refuses this to prevent circular
    dependencies.  
    **Fix:** Choose a solid that is not yet inside any Body, or use
    [Sub-Shape Binder](sub-shape-binder.md) to reference the existing Body's
    shape from the new Body.

---

## Python API

### Minimal example

```python
import FreeCAD as App

doc = App.newDocument()

# Create the Body
body = doc.addObject("PartDesign::Body", "Body")
doc.recompute()

print(body.Label)         # "Body"
print(body.Tip)           # None — no features yet
print(body.AllowCompound) # True
```

### Creating a Body with a base feature

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a Part box to use as the base solid
box = doc.addObject("Part::Box", "Box")
box.Length = 50
box.Width  = 30
box.Height = 20
doc.recompute()

# Create the Body and attach the box as its Base Feature
body = doc.addObject("PartDesign::Body", "Body")
body.BaseFeature = box
doc.recompute()

print(body.Tip.Label)  # "BaseFeature" proxy created automatically
```

### Key methods

```python
# Add an already-created feature (sketch, datum, solid feature) to the Body.
# The feature is inserted at the current insert point (just after the Tip).
body.addObject(feature)

# Insert a feature at a specific position relative to an existing feature.
# target: the reference feature (must already be in the Body); None → end/start
# after: if True, insert after target; if False (default), insert before target
body.insertObject(feature, target, after=False)

# Remove a feature from the Body without deleting it from the document.
body.removeObject(feature)

# Return the currently visible PartDesign feature inside the Body (Python property).
visible = body.VisibleFeature  # TODO: verify attribute name; source: BodyPyImp.cpp::getVisibleFeature
```

### Body properties reference

| Property | Type | Description |
|----------|------|-------------|
| `Label` | `App::PropertyString` | Human-readable name of the Body. |
| `Tip` | `App::PropertyLink` | Link to the feature that determines the Body's current output shape. |
| `BaseFeature` | `App::PropertyLink` | Optional external solid that the first PartDesign feature builds upon. |
| `Group` | `App::PropertyLinkList` | Ordered list of all objects inside the Body. |
| `AllowCompound` | `App::PropertyBool` | `True` allows multiple disconnected solids in the result. Default: `True`. |
| `Placement` | `App::PropertyPlacement` | Position and orientation of the Body in world space. |
| `ShapeMaterial` | `App::PropertyMaterial` <!-- TODO: verify type name --> | Appearance material propagated to all contained features. |
| `Shape` | `Part::PropertyPartShape` | The output shape — a copy of the Tip feature's shape. Read-only; recomputed automatically. |

---

## See also

- [Part Design index](../index.md) — overview of the Part Design workflow
- [Pad](pad.md) — the most common first feature to add inside a Body
- [New Sketch](new-sketch.md) — create the sketch that a Pad or Pocket uses <!-- TODO: verify link -->
- [Move Tip](move-tip.md) — move the active Tip to inspect or insert at an intermediate step
- [Boolean](boolean.md) — combine two or more Bodies into a single solid
- [Sub-Shape Binder](sub-shape-binder.md) — reference geometry from another Body
- [Move Feature](move-feature.md) — reassign a feature from one Body to another
