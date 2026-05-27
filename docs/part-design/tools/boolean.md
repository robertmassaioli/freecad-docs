# Boolean Operation

> **In one sentence:** Combine the active Body with one or more other Bodies using a
> union, subtraction, or intersection to produce a single result solid.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Boolean Operation  
**Toolbar:** Part Design Modeling Features · **Shortcut:** none

Boolean Operation merges two or more separate Part Design Bodies into one by
applying a classic set operation: Fuse (union), Cut (difference), or Common
(intersection). The result replaces the active Body's shape, consuming the
selected tool Bodies into the feature history. It is the Part Design equivalent
of the Boolean commands in the Part workbench, but it operates directly on
Bodies inside the parametric feature tree.

---

## Intuition

Imagine you have modelled two separate solid objects — a shaft and a keyway block.
To cut the keyway into the shaft you need to subtract one from the other. That is
exactly what Boolean Cut does: it chisels the second solid out of the first.

The three operations cover all the classic solid-set combinations:

- **Fuse** (union) — all material from both Bodies is kept. Use it to join a
  bracket arm to a base plate when the geometry is too complex to model as one
  sketch.
- **Cut** (difference) — material from the tool Bodies is removed from the base.
  Use it to drill irregular pockets whose shape is easier to model as a separate
  Body than as a Pocket sketch.
- **Common** (intersection) — only the volume that belongs to *every* Body is
  kept, discarding the rest. Use it to find the overlapping region of two
  interlocking parts.

Unlike the Part workbench Boolean commands, this tool lives in the Body's feature
tree, so the result is parametric: change the shape of either Body and the Boolean
result updates automatically.

---

## When to use it

- You have two Bodies in the same document and need to merge, subtract, or
  intersect them at the Part Design level without leaving the Body workflow.
- The geometry you want to remove (a complex pocket or an irregular boss) is
  easier to model as a full standalone Body than as a Sketcher profile used in
  a Pocket or Groove.
- You are assembling interlocking parts and need to compute the common volume to
  check for clashes.
- You want to join additive features from different Bodies while keeping both
  Bodies parametric (rather than creating a manual copy).

---

## When NOT to use it

- **The two shapes are in the same Body** — use [Pad](pad.md),
  [Pocket](pocket.md), or [Groove](groove.md) instead; adding or removing
  material within one Body does not require Boolean Operation.
- **You only need to fuse touching faces to get cleaner topology** — enable the
  **Refine** toggle on the Boolean feature (or on any other feature) instead of
  creating an extra Boolean step.
- **You want to operate on imported STEP shapes that are not Bodies** — use the
  Part workbench Boolean tools (Part → Boolean) instead, which accept arbitrary
  shapes.
- **You want to create a compound of non-intersecting solids** — use
  Part → Make Compound (Part workbench) instead.

---

## Step-by-step walkthrough

**Prerequisites:** An active document containing an active Body (the *base*) and
at least one other Body (the *tool*). Both Bodies must contain valid geometry.

1. **Activate the base Body** by double-clicking it in the model tree. The Body
   that will be modified is the active one.

2. **Open Boolean Operation** via Part Design → Boolean Operation, or click the
   **Boolean** button in the Part Design Modeling Features toolbar.  
   The **Boolean Parameters** task panel opens.

3. **Choose the operation type.** In the *Type* dropdown at the bottom of the
   panel, select one of:
   - **Fuse** — adds material from the tool Bodies to the base.
   - **Cut** — removes material of the tool Bodies from the base.
   - **Common** — keeps only the intersecting volume.

4. **Add tool Bodies.** Click **Add Body**, then click each Body you want to use
   as a tool in the model tree or 3D viewport. Each selected Body appears in the
   list widget. Repeat until all tool Bodies are listed.

    !!! warning "Cut requires a base shape"
        If the base Body has no features yet (its shape is empty), the Cut
        operation will fail with "Cannot do boolean cut without BaseFeature".
        Make sure the base Body contains at least one feature before applying Cut.

5. **Remove unwanted entries** by right-clicking a list entry and choosing
   **Remove**, or by clicking **Remove Body** and clicking the Body in the
   viewport.

6. **Click OK.** FreeCAD runs the Boolean operation and updates the base Body's
   shape. The tool Bodies are hidden but remain in the model tree and stay
   parametric.

!!! tip
    If you pre-select one or more Bodies in the model tree *before* activating
    Boolean Operation, FreeCAD adds them to the list automatically, saving the
    Add Body step.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Add Body | Button | — | Enters selection mode; the next Body you click is appended to the tool list. |
| Remove Body | Button | — | Enters removal mode; the next Body you click is removed from the tool list. |
| Body list | List widget | (empty) | Shows all Bodies currently registered as tools. Right-click for a context menu with a Remove option. |
| Type | Dropdown | `Fuse` | The Boolean operation to apply. Values: `Fuse` (union), `Cut` (difference), `Common` (intersection). |
| Refine | Toggle (inherited) | (global setting) | When enabled, removes redundant coplanar faces from the result to produce cleaner topology. Inherited from `FeatureRefine`. |

---

## Advanced usage

### Multiple tool Bodies

More than one Body can be listed as a tool. For Fuse, all tool Bodies are merged
into the base simultaneously. For Cut, all tool Bodies are subtracted from the
base in sequence. For Common, the result is the intersection of all Bodies
together.

### Parametric updates

Because both the base Body and the tool Bodies remain in the model tree,
changing a dimension in either will propagate through the Boolean feature on
recompute. This makes Boolean Operation suitable for parametric design
where the overlap or gap between components must update automatically.

### Allow Compound setting

By default, FreeCAD enforces a *single-solid rule*: the Boolean result must be
one connected solid. If the operation would produce multiple disjoint solids
(for example, a Cut that splits the base into two pieces), the feature reports
an error: "Result has multiple solids: enable 'Allow Compound' in the active
body." To allow multi-solid results, open the Body's properties and set
**Allow Compound** to `True`.

---

## Common mistakes and pitfalls

!!! warning "Tool list is empty on OK"
    **Cause:** You clicked OK without adding any Body to the tool list.  
    **Fix:** The dialog blocks acceptance and shows a warning. Click Add Body and
    select at least one Body before clicking OK.

!!! warning "Boolean operation failed (OCCT error)"
    **Cause:** The two solids share only a face, an edge, or a single point rather
    than overlapping volume, or the geometry is too close to tolerance limits.  
    **Fix:** Ensure the solids genuinely overlap (for Fuse/Cut) or genuinely
    intersect with volume (for Common). Move one Body slightly if the faces are
    exactly flush.

!!! warning "Result has multiple solids"
    **Cause:** A Cut operation severed the base solid into two or more disconnected
    pieces, violating the single-solid rule enforced by the active Body.  
    **Fix:** Either redesign the cut so that it does not fully sever the base, or
    enable **Allow Compound** in the Body's properties (see Advanced usage above).

!!! warning "Cannot do boolean cut without BaseFeature"
    **Cause:** The active Body contains no features, so there is no base shape to
    cut from.  
    **Fix:** Add at least one additive feature (e.g. a Pad) to the base Body
    before applying Boolean Cut.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign  # TODO: verify — may not need explicit import

doc = App.newDocument()

# Create base body with a box-shaped pad
base_body = doc.addObject("PartDesign::Body", "BaseBody")
# (assume base_body already has geometry via a Pad or similar)

# Create tool body (the cutter)
tool_body = doc.addObject("PartDesign::Body", "ToolBody")
# (assume tool_body already has geometry)

# Add Boolean feature to base body
boolean = base_body.newObject("PartDesign::Boolean", "Boolean")
boolean.Type = "Cut"         # "Fuse", "Cut", or "Common"
boolean.addObjects([tool_body])
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Type` | `App::PropertyEnumeration` | The Boolean operation. Values: `"Fuse"`, `"Cut"`, `"Common"`. Default: `"Fuse"`. |
| `Group` | `App::PropertyLinkList` | The list of tool Bodies (formerly named `Bodies` in older FreeCAD versions). |
| `Refine` | `App::PropertyBool` | Whether to clean up redundant faces after the operation. Inherited from `FeatureRefine`. |

---

## See also

- [Pad](pad.md) — additive extrusion within a single Body
- [Pocket](pocket.md) — subtractive extrusion within a single Body
- Part Boolean (Part workbench) — Part workbench equivalent that
  operates on arbitrary shapes rather than Bodies
- [Body](body.md) — the Part Design container that Boolean
  operates on
