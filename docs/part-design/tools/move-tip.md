# Set Tip

> **In one sentence:** Designate any feature in the body's history as the active
> endpoint, suppressing all features that come after it without deleting them.

---

## Overview

**Workbench:** Part Design · **Menu:** *(context menu only — right-click a feature in the model tree)*  
**Toolbar:** none · **Shortcut:** none

"Move Tip" is the common informal name for the **Set Tip** command
(`PartDesign_MoveTip`). The *Tip* is the feature that the Body considers its
final result — the shape shown in the 3-D viewport. Moving the Tip backward in
the tree suppresses every feature after it from the computed solid while leaving
those features intact and ready to be re-enabled. This makes it straightforward
to insert new features between existing ones.

---

## Intuition

Think of the feature tree as a recipe for a part, read from top to bottom. Each
step adds or removes material. Normally, FreeCAD reads the entire recipe and
shows the final dish. The Tip marker tells FreeCAD where to stop reading. If you
place the Tip halfway through the recipe, only the steps up to that point are
cooked into the solid; the rest are held in reserve.

This is useful for inserting a new ingredient into the middle of an existing
recipe: move the Tip back to just before the insertion point, add the new step
(which now lands at the Tip), then restore the Tip to the end of the tree. Every
subsequent step is automatically recomputed with the new feature included.

Unlike deleting a feature, changing the Tip is completely non-destructive. The
suppressed features are still in the tree and will be reincluded as soon as the
Tip advances past them again.

---

## When to use it

- You need to insert a new feature (a Pad, a Pocket, a datum plane) between two
  existing features without rebuilding the model.
- You want to temporarily inspect what the body looked like at an earlier stage
  of modelling, without deleting later work.
- You are troubleshooting a model failure and want to isolate which feature
  introduced an error by stepping the Tip backwards.
- You have accidentally added a feature at the end of the tree when you meant to
  add it earlier, and you want subsequent features to build on top of it.

---

## When NOT to use it

- **You want to permanently stop the body at a given feature** — the Tip is a
  runtime state, not a structural change. It survives save and reload, but it is
  not the same as deleting features.
- **You want to change the evaluation order of features** — use
  [Move Feature After…](move-feature-in-tree.md) instead; that reorders features
  in the tree, whereas Set Tip only changes where the tree stops evaluating.
- **You want to move a feature to a different Body** — use
  [Move Object To…](move-feature.md) instead.
- **You want to suppress a feature without changing the tip** — suppress
  individual features via their context menu (right-click → Toggle Active Feature)
  if available <!-- TODO: verify command name -->.

---

## Step-by-step walkthrough

**Prerequisite:** A Body with at least two features in the model tree.

1. In the **Model** panel (model tree), right-click the feature you want to
   become the new Tip.

2. In the context menu, click **Set Tip**.

3. The chosen feature is now marked as the Tip (bold or underlined in the tree,
   depending on your FreeCAD theme). All features listed after it in the tree
   are suppressed — they remain visible in the tree but no longer contribute to
   the solid shape.

4. The 3-D viewport updates immediately to show the body as it was at the point
   of the new Tip.

!!! tip
    To restore the Tip to the last feature in the tree (the most common next
    step after inserting a new feature), right-click the last feature and choose
    **Set Tip** again.

!!! note
    You can also set the Tip to the Body object itself. This clears the Tip
    (`Tip = None`), resulting in no solid being displayed. This is mainly useful
    in scripting.

---

## Parameters

Set Tip has no task panel or dialog. It is a single-click action applied to the
selected feature. The only "parameter" is the feature you select before
right-clicking.

| Input | Description |
|-------|-------------|
| Selected feature | The Part Design feature (or the Body itself) that becomes the new Tip. Must be a solid feature belonging to the body; datum objects cannot be the Tip. |

---

## Advanced usage

### Inserting a feature mid-tree

The typical workflow for inserting a new feature between existing ones is:

1. Move the Tip to the feature immediately *before* the intended insertion point.
2. Create the new feature (e.g. a new Sketch and Pad). FreeCAD always adds new
   features directly after the current Tip, so the new feature lands in the
   correct position.
3. Move the Tip back to the last feature in the tree to restore the full model.

### Tip and visibility

When you set a new Tip, FreeCAD makes that feature visible in the viewport and
hides the previously visible shape. Datum features after the new Tip are not
automatically hidden. You can manually toggle their visibility if the scene
becomes cluttered.

---

## Common mistakes and pitfalls

!!! warning "Context menu entry does not appear"
    **Cause:** The selected object is not a Part Design solid feature, or it
    belongs to a different Body than the active one. Datum objects (planes,
    lines, points) cannot be the Tip.  
    **Fix:** Select a Pad, Pocket, Revolution, or similar solid feature inside
    the active Body before right-clicking.

!!! warning "Model looks empty after setting the Tip"
    **Cause:** The Tip was set to the Body object itself, which clears the Tip
    (`Tip = None`) and produces no shape.  
    **Fix:** Right-click the first solid feature in the tree and choose **Set
    Tip** to restore a visible result.

!!! warning "New features land at the wrong position in the tree"
    **Cause:** After moving the Tip backward to insert a feature, you forgot to
    move the Tip back to the end of the tree. Subsequent features continue to be
    added immediately after the current Tip position.  
    **Fix:** After inserting the new feature, right-click the last feature in
    the full tree and choose **Set Tip** to restore the normal end-of-tree
    position.

---

## Python API

Set Tip is exposed through the `Tip` property on a `PartDesign::Body` object.
There is no dedicated Python function — you assign the property directly.

### Minimal example

```python
import FreeCAD as App

doc = App.activeDocument()
body = doc.getObject("Body")  # replace with your Body's name

# Move the Tip to a specific feature
feature = doc.getObject("Pad")  # replace with your feature's name
body.Tip = feature
doc.recompute()

# Clear the Tip (no solid displayed)
body.Tip = None
doc.recompute()
```

### Body property

| Property | Type | Description |
|----------|------|-------------|
| `Tip` | `App::PropertyLink` | The feature that is the active endpoint of the body. Set to a feature object to move the Tip, or `None` to clear it. |

---

## See also

- [Move Feature After…](move-feature-in-tree.md) — reorder a feature's position within the same Body
- [Move Object To…](move-feature.md) — move a feature from one Body to another
- [Duplicate Object](duplicate-selection.md) — copy a feature within the active Body
- [Body and Origin](../concepts/body-and-origin.md) — how Bodies, features, and the Tip relate
