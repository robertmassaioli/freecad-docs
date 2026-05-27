# Duplicate Object

> **In one sentence:** Copy selected features or objects into the active Body
> as fully independent duplicates with no parametric link back to the originals.

---

## Overview

**Workbench:** Part Design · **Menu:** Edit → Duplicate Object  
**Toolbar:** none · **Shortcut:** none

When the Part Design workbench is active, the standard **Edit → Duplicate
Selection** menu item is replaced by **Edit → Duplicate Object**
(`PartDesign_DuplicateSelection`). This PartDesign-aware variant wraps the
standard `Std_DuplicateSelection` operation and then automatically inserts
the newly created objects into the currently active Body.

The result is one or more deep copies of the selected objects. The copies share
no parametric link with the originals: changing the original feature does not
affect the duplicate, and vice versa. All properties — sketch geometry,
dimensions, references — are copied at the moment of duplication and then
become entirely independent.

---

## Intuition

Imagine photocopying a page of technical drawings. The photocopy looks identical
to the original at the moment it is made, but writing on the photocopy does not
change the original and any later changes to the original do not appear on the
photocopy. Duplicate Object works the same way: you get a snapshot of the
selected features, frozen in time, placed in the active Body.

This is the opposite of [Clone](clone.md), which creates a *live link* that
always reflects the current state of the source. Duplicate Object creates a
*dead copy* that you are free to edit independently.

---

## When to use it

- You want to start a design variant from the current state of a feature or
  Body, keeping the original untouched and making independent modifications to
  the copy.
- You want to reuse a sketch as the starting point for a second, similar
  sketch, without the second sketch being linked to the first.
- You are copying a feature into a different Body that has similar geometry
  requirements but different final dimensions.
- You need a fallback snapshot of the current feature state before attempting
  a risky edit.

---

## When NOT to use it

- **You want the copy to stay synchronized with the original** — use
  [Clone](clone.md) instead; it creates a live parametric link.
- **You want to reuse geometry from another Body without copying it** — use
  [Shape Binder](shape-binder.md) or [Sub-Shape Binder](sub-shape-binder.md),
  which reference the original geometry without duplicating it.
- **You want to move a feature between Bodies without copying** — use
  [Move Object To…](move-feature.md) instead.
- **You want to move a feature to a different position in the same Body's
  history** — use [Move Object in Tree](move-feature-in-tree.md) instead.

---

## Step-by-step walkthrough

**Prerequisites:** At least one object must be selected in the model tree, and
a Body must be active (for the duplicate to be inserted into it automatically).

1. **Activate the target Body.** Double-click the Body in the model tree so it
   appears bold. If no Body is active the duplicate is still created, but it
   will not be inserted into a Body automatically.

2. **Select the feature(s) to duplicate.** Click the feature in the model tree.
   Multiple features can be selected with Ctrl+click.

3. **Launch the command.** Choose **Edit → Duplicate Object** from the menu.
   Because the PartDesign workbench is active, this invokes
   `PartDesign_DuplicateSelection` rather than the standard
   `Std_DuplicateSelection`.

4. **Inspect the result.** New objects with the same names (plus a numeric
   suffix to make them unique) appear in the model tree. If a Body was active,
   they are placed inside that Body. The last new object is made visible; all
   others are hidden.

5. **Edit the duplicate independently.** Double-click the duplicated feature to
   open its task panel and change dimensions, add constraints, or otherwise
   modify it. The original remains unchanged.

!!! tip
    If you duplicate a Sketch, the duplicate is a completely independent
    `Sketcher::SketchObject`. Open it and you will find all the geometry and
    constraints present, but with no link back to the source sketch. You can
    freely add, remove, or change constraints.

!!! tip
    After duplication the duplicate features' `Placement` values are identical
    to the originals. If you are duplicating into the same Body, consider
    adjusting positions or sketch attachment offsets to avoid the duplicate
    sitting exactly on top of the original.

---

## Parameters

Duplicate Object has no interactive parameter panel — it executes immediately
on invocation. The resulting objects inherit all properties from the originals.
After duplication, each property is editable independently in the *Properties*
panel (View → Panels → Properties).

### Behaviour details

| Aspect | Value / Description |
|--------|---------------------|
| Link to original | **None.** The duplicate is fully independent. |
| Property values | Copied from the original at the time of duplication. |
| Sketch geometry | All points, edges, and constraints are deep-copied. External references in the sketch (links to Body geometry) may need to be re-established if the duplicate is in a different Body context. |
| Feature name | Same base name as the original plus a numeric suffix for uniqueness (e.g. `Pad` → `Pad001`). |
| Visibility | The last duplicated object is shown; all others are hidden. |
| Body insertion | If a Body is active, all new `PartDesign`-allowed features are added to that Body. Features that are already in a Body are not moved (issue #6278). |
| Undo | The entire duplication is wrapped in a single undo transaction labelled *"Duplicate a Part Design object"*. |

---

## Advanced usage

### Duplicating into a different Body

Activate the *destination* Body before invoking the command. The new features
are inserted into the active Body. If the source feature was in a different Body
and contained references to that Body's geometry (for example, a sketch with an
external edge constraint), those references will be broken in the copy and must
be re-established manually.

### Duplicating multiple features at once

Select several features in the model tree (Ctrl+click) before invoking the
command. All selected features are duplicated in a single undo transaction.
Note that if features have dependencies on each other (for example, a Pad that
uses a Sketch), select both the Sketch and the Pad to avoid the duplicate Pad
referencing the *original* Sketch.

### Using duplication for design variants

A common workflow:

1. Complete a base feature to a known-good state.
2. Duplicate the feature.
3. Rename the duplicate (right-click → Rename) to reflect the variant.
4. Edit the duplicate's parameters for the new variant.

Both the original and the variant coexist in the model tree. You can switch the
Body's `Tip` property to expose one or the other as the Body's output shape.

### Relationship to Std_DuplicateSelection

`PartDesign_DuplicateSelection` calls `FreeCADGui.runCommand('Std_DuplicateSelection')`
internally. The actual deep-copy logic lives in the standard command. The
PartDesign wrapper adds the post-processing step that detects which new objects
were created and inserts them into the active Body. If no Body is active the
behaviour is identical to the standard command.

---

## Common mistakes and pitfalls

!!! warning "External references in duplicated sketches become broken"
    **Cause:** When a Sketch in the original Body has *External Geometry*
    constraints (red reference edges imported from the Body solid), those
    references point to specific sub-shapes by topological name. In a different
    Body context the same names may not exist.  
    **Fix:** After duplication, open the sketch, delete the broken external
    geometry (shown in red or yellow), and re-attach it to the corresponding
    geometry in the new Body.

!!! warning "Duplicate placed exactly on top of original, hard to distinguish"
    **Cause:** The duplicate inherits the same `Placement` as the original.
    For 3D features inside the same Body, they occupy the same space until
    you edit their parameters.  
    **Fix:** Either immediately open the duplicate's task panel and change a
    dimension, or set an `AttachmentOffset` on the duplicate's sketch to shift
    it to a different position.

!!! warning "Duplicate is not inserted into the active Body"
    **Cause:** The feature is already a member of another Body.
    `PartDesign_DuplicateSelection` skips features that already belong to a
    Body (issue #6278) to avoid moving them unintentionally.  
    **Fix:** If you want the duplicate inside the current Body, use
    [Move Object To…](move-feature.md) on the duplicate after it is created,
    or copy the feature manually via the Properties panel.

!!! warning "Duplicating a Body duplicates its entire feature tree"
    **Cause:** `Std_DuplicateSelection` deep-copies the selected object and all
    its children. Selecting a Body copies every feature inside it, which may
    create a very large number of new objects.  
    **Fix:** Select only the specific features you need, not the Body container
    itself, unless you genuinely want a full copy of the Body.

---

## Python API

Duplicate Object does not have a dedicated Python class — it invokes the
standard `Std_DuplicateSelection` command through the GUI command system. To
duplicate programmatically, use `FreeCAD.ActiveDocument.copyObject()`:

```python
import FreeCAD as App
import FreeCADGui as Gui

doc = App.activeDocument()

# Get the feature to duplicate
source = doc.getObject("Pad")

# Deep-copy the object (with = True means copy children too)
duplicate = doc.copyObject(source, True)

# If a Body is active, add the duplicate to it
body = doc.getObject("Body")
if body and hasattr(duplicate, "isDerivedFrom"):
    import PartDesign
    if PartDesign.Body.isAllowed(duplicate):
        body.addObject(duplicate)

doc.recompute()
print(duplicate.Label)   # e.g. "Pad001"
```

To invoke the command exactly as the menu item does (including the Body
insertion logic):

```python
import FreeCADGui as Gui

# Select the features you want to duplicate first
# Gui.Selection.addSelection(...)

Gui.runCommand("PartDesign_DuplicateSelection")
```

### Key behaviour of the underlying copy mechanism

| Aspect | Description |
|--------|-------------|
| `doc.copyObject(obj, True)` | Deep-copies `obj` and all its children into the same document. Returns the top-level copy. |
| Property values | Scalar, string, and placement properties are value-copied. Link properties may still point to original objects — check and update as needed. |
| Label | Assigned automatically by FreeCAD to ensure uniqueness (usually base name + numeric suffix). |
| Name | Also made unique, following FreeCAD's internal naming rules. |

---

## See also

- [Clone](clone.md) — creates a live parametric link instead of an independent
  copy
- [Move Object To…](move-feature.md) — moves a feature to a different Body
  without copying it
- [Move Object in Tree](move-feature-in-tree.md) — reorders a feature within
  the same Body's history
- [Shape Binder](shape-binder.md) — references geometry from another Body
  without copying it
