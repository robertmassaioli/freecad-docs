# Move Object To…

> **In one sentence:** Transfer one or more features from their current Body to a
> different Body in the same document.

---

## Overview

**Workbench:** Part Design · **Menu:** *(context menu only — right-click a feature in the model tree)*  
**Toolbar:** none · **Shortcut:** none

"Move Feature" is the common informal name for the **Move Object To…** command
(`PartDesign_MoveFeature`). It transfers the selected feature (or a selection of
features) out of their source Body and into a target Body that you choose from a
dialog. Any features that the selection depends on are automatically detected and
moved along with it. After the move, the target Body is updated to include the
transferred features at its Tip.

---

## Intuition

A Part Design document often contains multiple Bodies, each representing a
distinct solid component. If you create a feature in the wrong Body by mistake,
or if you later decide a feature belongs with a different component, Move Object
To… lets you correct the assignment without deleting and recreating anything.

Think of it as physically lifting a building block out of one stack and dropping
it on top of another stack. The block itself does not change — its geometry,
sketch constraints, and parameters are preserved — but it now contributes to the
other Body's result.

Because features often rely on other features (a Pad needs a Sketch; a Pocket
may reference a datum plane), the command automatically collects all required
dependencies and moves them as a group. If a dependency cannot be moved — for
example, because it is referenced by features that are staying behind — the
command refuses to proceed and shows an error.

---

## When to use it

- You accidentally created a feature inside the wrong Body and want to move it
  to the correct one without rebuilding it.
- You are reorganising a multi-body assembly and need to redistribute features
  between components.
- You created a sketch and a Pad in Body A as a quick test and now want that
  solid to live in Body B.
- You are splitting a complex Body into two simpler Bodies and need to redistribute
  the feature history.

---

## When NOT to use it

- **You want to reorder a feature within the same Body** — use
  [Move Feature After…](move-feature-in-tree.md) instead; Move Object To… only
  transfers between Bodies, not within one.
- **You want to change where the Body stops evaluating** — use
  [Set Tip](move-tip.md) instead.
- **You want a copy rather than a move** — use
  [Duplicate Object](duplicate-selection.md) to create a copy first, then move
  the copy if needed.
- **The target document has no other Body** — the command requires at least one
  Body in the document other than the source Body. Create a second Body first.

---

## Step-by-step walkthrough

**Prerequisite:** A document containing at least two Bodies. The feature you
want to move must belong to one of them.

1. In the **Model** panel (model tree), select the feature or features you want
   to move. You can select multiple features using Ctrl-click.

2. Right-click the selection and choose **Move Object To…** from the context
   menu.

    !!! warning "Entry not visible"
        The context menu entry only appears when right-clicking inside the model
        tree (`Tree` context), and only when the document contains at least one
        Body other than the source Body.

3. A dialog appears listing all valid target Bodies by name. Select the
   destination Body and click **OK**.

4. The features are removed from the source Body and added to the end of the
   target Body (at the target Body's Tip).

5. The 3-D viewport updates. The source Body loses the transferred geometry; the
   target Body gains it.

!!! tip
    If the moved features reference sketch planes or datum objects that must move
    with them, the command detects these dependencies automatically and includes
    them in the transfer. Review the target Body's feature tree after the move to
    confirm that all expected objects arrived.

---

## Parameters

Move Object To… has no task panel. After selecting features and invoking the
command, a single dialog appears.

| Input | Description |
|-------|-------------|
| Target Body (dropdown list) | The Body to receive the features. Only Bodies in the same document that are not the source Body are listed. |

---

## Advanced usage

### Dependency resolution

The command calls `PartDesignGui::collectMovableDependencies` to find all objects
that the selected features depend on, then moves the entire set together. This
means that if you select a Pad, its underlying Sketch is moved too (if the Sketch
does not have other consumers that remain in the source Body).

Before moving, each feature is checked with `PartDesignGui::isFeatureMovable`.
If any selected feature has dependencies that remain in the source Body — for
example, the sketch references a face of another feature that is staying behind
— the move is blocked and a warning is shown.

### Moving features that are not yet in a Body

If a Part Design feature exists in the document but is not yet assigned to any
Body, Move Object To… can still place it into a target Body. This is useful when
features have been created or imported at document level without a Body context.

### Effect on the source Body's Tip

If the moved feature was the Tip of the source Body, the source Body's Tip is
updated automatically to the preceding feature. The exact new Tip depends on the
Body's internal order after the removal.

---

## Common mistakes and pitfalls

!!! warning "Features cannot be moved — dependencies in the source body"
    **Cause:** At least one selected feature references geometry that belongs to
    a feature remaining in the source Body (e.g. a Pocket sketched on a face of
    a Pad that is not being moved).  
    **Fix:** Either include all dependent features in the selection, or
    restructure the sketch to reference a datum plane instead of a body face
    before moving.

!!! warning "Only features of a single source body can be moved"
    **Cause:** The selection includes features from more than one Body.  
    **Fix:** Make a separate Move Object To… operation for each source Body.

!!! warning "There are no other bodies to move to"
    **Cause:** The document contains only one Body.  
    **Fix:** Create a second Body (Part Design → Body) before invoking Move
    Object To….

!!! warning "Sketch plane references broken after the move"
    **Cause:** The sketch was attached to a face of the source Body, and that
    reference no longer exists in the target Body.  
    **Fix:** After the move, edit the affected sketch and reattach it to a datum
    plane or a face of the target Body.

---

## Python API

Move Object To… is implemented by calling `removeObjects` on the source Body and
`addObjects` on the target Body. There is no dedicated Python function for the
move operation — you call these Body methods directly.

### Minimal example

```python
import FreeCAD as App

doc = App.activeDocument()
source_body = doc.getObject("Body")   # replace with your source Body name
target_body = doc.getObject("Body001")  # replace with your target Body name

# Move a single feature
feature = doc.getObject("Pad")  # replace with your feature name
source_body.removeObjects([feature])  # TODO: verify method name is removeObjects
target_body.addObjects([feature])     # TODO: verify method name is addObjects
doc.recompute()
```

### Body methods

| Method | Description |
|--------|-------------|
| `removeObjects(features)` | Remove a list of feature objects from the Body's group. The features are not deleted from the document. <!-- TODO: verify exact signature --> |
| `addObjects(features)` | Add a list of feature objects to the Body. They are inserted at the Body's current Tip position. <!-- TODO: verify exact signature --> |

---

## See also

- [Move Feature After…](move-feature-in-tree.md) — reorder a feature within the same Body
- [Set Tip](move-tip.md) — change which feature is the active endpoint of a Body
- [Duplicate Object](duplicate-selection.md) — copy a feature rather than moving it
- [Body, Origin and Feature Tree](../concepts/body-and-origin.md) — how features are organised within a Body
