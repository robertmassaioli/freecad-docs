# Move Feature After…

> **In one sentence:** Change the position of a feature within a Body's feature
> tree so that it is evaluated after a different feature.

---

## Overview

**Workbench:** Part Design · **Menu:** *(context menu only — right-click a feature in the model tree)*  
**Toolbar:** none · **Shortcut:** none

"Move Feature in Tree" is the common informal name for the **Move Feature
After…** command (`PartDesign_MoveFeatureInTree`). It changes the order in which
features are evaluated within a single Body by repositioning the selected feature
(or features) after a target feature that you choose from a list. Because Part
Design evaluates features in tree order, changing that order changes the resulting
solid shape.

---

## Intuition

A Body's feature tree is an ordered sequence of operations. FreeCAD evaluates
them from top to bottom: each feature receives the shape produced by all features
before it and adds or removes material from it. If you place a Fillet before a
Pocket, the fillet is on a face that the pocket may later destroy. Placing the
Fillet after the Pocket changes the outcome.

Think of it as rearranging steps in a recipe. The ingredients (the features)
stay the same, but the order in which you apply them produces a different final
dish. Move Feature After… is the tool that lets you change that order after the
fact, without having to delete and recreate anything.

---

## When to use it

- You created a feature at the end of the tree but it logically belongs earlier —
  for example, a Fillet that should come before a subsequent Pocket.
- You want to reorder a datum plane or datum line so that it appears before the
  features that reference it, making the dependency chain explicit.
- You need to fix evaluation order after copying or importing features that
  arrived in the wrong sequence.
- You want to group related features visually in the tree, even when the reorder
  does not change the geometry (e.g. moving datum planes together at the top of
  the tree).

---

## When NOT to use it

- **You want to move a feature to a different Body** — use
  [Move Object To…](move-feature.md) instead; Move Feature After… only works
  within the same Body.
- **You want to suppress all features after a given point** — use
  [Set Tip](move-tip.md) instead.
- **You want a copy rather than a reorder** — use
  [Duplicate Object](duplicate-selection.md) to create a copy at a new position.
- **The BaseFeature (the imported solid at the base of the Body) needs to be
  moved** — the command explicitly blocks this; the BaseFeature must always remain
  at the start of the tree.

---

## Step-by-step walkthrough

**Prerequisite:** A Body with at least two features in the model tree.

1. In the **Model** panel (model tree), select the feature or features you want
   to reorder. You can Ctrl-click to select multiple features.

    !!! warning "All selected features must belong to the same Body"
        If any selected feature is in a different Body, the command shows an
        error and does nothing.

2. Right-click the selection and choose **Move Feature After…** from the context
   menu.

3. A dialog appears listing all features currently in the Body, plus a special
   entry **"Beginning of the body"** at the top of the list. Select the feature
   you want the moved feature to follow, then click **OK**.

    - Choosing **"Beginning of the body"** places the feature at the very start
      of the tree (before all other features).
    - Choosing any named feature places the moved feature immediately after it.

4. FreeCAD performs a dependency-order check. If moving the feature would place
   it before another feature that it depends on — violating causality — the
   command is aborted and a **Dependency violation** dialog describes the
   conflict.

5. If the moved feature lands after the current Tip, a confirmation dialog asks:
   **"Set tip to last feature?"**. Clicking **Yes** advances the Tip to the
   moved feature; clicking **No** leaves the Tip unchanged.

6. The model tree reflects the new order and FreeCAD recomputes the Body.

!!! tip
    When reordering multiple features at once, they are inserted in their
    original relative order immediately after the chosen target feature. Their
    sequence relative to each other is preserved.

---

## Parameters

Move Feature After… has a single input: the position within the tree where the
feature should be placed.

| Input | Description |
|-------|-------------|
| Target position (dropdown list) | The feature after which the selected feature(s) will be inserted. The list includes every feature in the Body plus "Beginning of the body" as the first entry. |

---

## Advanced usage

### What counts as a valid feature to move

The command accepts:

- Any `PartDesign::Feature` (Pad, Pocket, Fillet, etc.)
- `App::DatumElement` objects (datum planes, datum lines, datum points)
- `App::LocalCoordinateSystem` objects (coordinate systems)

It does not accept the Body's BaseFeature (the external solid, if any, that the
Body is built upon).

### Dependency-order enforcement

After the reorder, FreeCAD checks every `PartDesign::Feature` in the new tree
order. If any feature depends on an object that appears later in the tree — a
dependency violation — the command is rolled back entirely and the tree is
restored to its previous state. The error dialog lists the specific feature
pairs that conflict.

This means you cannot, for example, move a Pad before the Sketch it extrudes.
You would need to move both the Sketch and the Pad together.

### Interaction with the Tip

If the moved feature ends up after the current Tip, FreeCAD offers to advance
the Tip to cover the moved feature. This is the expected behaviour when you are
inserting a late-arriving feature and want it included in the final shape. You
can decline and manually adjust the Tip using [Set Tip](move-tip.md) instead.

---

## Common mistakes and pitfalls

!!! warning "Dependency violation — Early feature must not depend on later feature"
    **Cause:** The reorder would place a feature before something it references.
    For example, moving a Pocket before the Pad whose face it uses.  
    **Fix:** Include all dependent objects in the selection so they move as a
    group, or restructure the sketch to reference a datum plane instead of a
    body face before reordering.

!!! warning "Impossible to move the base feature of a body"
    **Cause:** The Body's BaseFeature (an imported solid at position 0 of the
    tree) was included in the selection.  
    **Fix:** Deselect the BaseFeature and reorder only the features above it.

!!! warning "Select one or more features from the same body"
    **Cause:** The selection includes features from different Bodies, or includes
    an object that is not inside any Body.  
    **Fix:** Select only features that belong to the same Body before invoking
    the command.

!!! warning "Reorder silently changes the shape without an obvious error"
    **Cause:** Moving a feature that modifies geometry which later features also
    modify can change the final solid shape in non-obvious ways. The model
    recomputes without error but produces a different result.  
    **Fix:** After reordering, inspect the 3-D viewport carefully and step
    through the tree using [Set Tip](move-tip.md) to verify that each stage
    still produces the intended geometry.

---

## Python API

Move Feature After… uses the `removeObject` and `insertObject` methods on the
Body. There is no dedicated Python function — you call these methods directly.

### Minimal example

```python
import FreeCAD as App

doc = App.activeDocument()
body = doc.getObject("Body")  # replace with your Body name

feature = doc.getObject("Fillet")       # the feature to move
after   = doc.getObject("Pad")          # insert after this feature

# Remove the feature from its current position
body.removeObject(feature)  # TODO: verify method name is removeObject

# Re-insert it after the target feature
# The third argument (True) inserts *after* the given object
body.insertObject(feature, after, True)  # TODO: verify signature and third-argument semantics

doc.recompute()
```

### Body methods

| Method | Description |
|--------|-------------|
| `removeObject(feature)` | Remove the feature from the Body's ordered group without deleting it from the document. <!-- TODO: verify exact signature --> |
| `insertObject(feature, after, after_flag)` | Re-insert the feature into the Body's group. When the third argument is `True`, the feature is placed *after* the specified object. When `False` or omitted, placement semantics differ. <!-- TODO: verify exact signature and flag meaning --> |

---

## See also

- [Set Tip](move-tip.md) — change where the Body stops evaluating, without reordering
- [Move Object To…](move-feature.md) — move a feature to a different Body
- [Duplicate Object](duplicate-selection.md) — copy a feature to a new position
- [Body, Origin and Feature Tree](../concepts/body-and-origin.md) — how features are ordered and evaluated in a Body
