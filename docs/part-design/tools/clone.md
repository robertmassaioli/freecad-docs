# Clone

> **In one sentence:** Wrap any solid object in a new Body as a parametric
> linked copy that stays synchronized with the original whenever it changes.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Clone  
**Toolbar:** Part Design Helper Features · **Shortcut:** none

Clone takes any `Part::Feature`-derived solid — a Body tip, a Part primitive, an
imported STEP solid — and creates a fresh `PartDesign::Body` containing a single
`PartDesign::FeatureBase` whose `BaseFeature` property links back to the source
object. Because the link is parametric, the clone recomputes every time the
source changes shape or placement. You get a separate Body with its own model
tree entry, its own visual appearance, and the ability to add further PartDesign
features on top of the imported shape.

---

## Intuition

Imagine you have a complex casting imported from STEP. You want to add machined
features (holes, fillets, chamfers) without touching the original import. Clone
creates an exact mirror of that casting inside a new Body. The mirror always
shows the current state of the casting. You then add features to the mirror;
the original import stays pristine.

Alternatively, if you are building an assembly of identical parts, Clone lets
you reuse one Body's geometry in several places while keeping all instances in
sync. Change the master, and every clone updates.

---

## When to use it

- You want to use a solid from outside the PartDesign workflow (a Part
  primitive, a Shape Binder result, or an imported solid) as the starting point
  for PartDesign features.
- You need multiple separate Bodies that share the same base geometry but have
  different additional features on top.
- You want to hand off the cleaned-up version of an imported STEP body to
  another Body for downstream machining operations without modifying the import.
- You need to place the same geometry in multiple positions with individual
  placements while each instance tracks the master shape.

---

## When NOT to use it

- **You want to reuse geometry from another Body within the same Body** — use
  [Shape Binder](shape-binder.md) or [Sub-Shape Binder](sub-shape-binder.md)
  instead; those tools bring references into an existing Body rather than
  creating a new one.
- **You want a fully independent copy with no link to the original** — use
  [Duplicate Object](duplicate-selection.md) instead; it produces an unlinked
  copy.
- **You want to pattern features inside a single Body** — use
  [Linear Pattern](linear-pattern.md), [Polar Pattern](polar-pattern.md), or
  [Mirrored](mirrored.md) instead.

---

## Step-by-step walkthrough

**Prerequisites:** At least one `Part::Feature`-derived solid must exist in the
document (a Body, a Part primitive, an imported solid, etc.).

1. **Select the source object.** Click the solid you want to clone in the model
   tree or the 3D viewport. The Clone command requires exactly one
   `Part::Feature` to be selected; the toolbar button is greyed out otherwise.

2. **Launch the command.** Click the **Clone** button in the *Part Design
   Helper Features* toolbar, or choose **Part Design → Clone** from the menu.

3. **Inspect the result.** Two new objects appear in the model tree: a new
   `Body` and a `Clone` (a `PartDesign::FeatureBase`) inside it. The clone's
   `BaseFeature` property points to your original object. FreeCAD copies the
   visual appearance (colour, transparency, display mode) from the source to
   the clone.

4. **Continue modelling.** Activate the new Body by double-clicking it. You can
   now add Pad, Pocket, Fillet, or any other PartDesign feature on top of the
   cloned shape exactly as if you had built it from scratch.

!!! tip
    If the source object has a complex colour or material assigned, the Clone
    command automatically copies `ShapeAppearance`, `LineColor`,
    `PointColor`, `Transparency`, and `DisplayMode` to the new Body — you do
    not need to re-apply them manually.

!!! tip
    You can rename both the generated `Body` and `Clone` objects immediately
    after creation. Right-click → **Rename** in the model tree so that the
    tree stays readable when you have many clones.

---

## Parameters

The Clone object (`PartDesign::FeatureBase`) has a small set of directly
editable properties. Open them in the *Properties* panel (View → Panels →
Properties).

### FeatureBase properties

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `BaseFeature` | `App::PropertyLink` | source object | The object whose shape this clone tracks. Changing this link retargets the clone to a different source. The link scope is **Global**, so cross-document references are supported. |
| `Placement` | `App::PropertyPlacement` | copied from source | Position and orientation of the clone in the world. Editable independently of the source — moving the clone does not move the source. The editor mode is set to `0` (visible and editable) by the Clone command. |

### Body properties (set by the Clone command)

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `Tip` | `App::PropertyLink` | the Clone feature | The feature whose shape the Body exposes. |
| `AllowCompound` | `App::PropertyBool` | from user preferences | When `True`, the Body may contain more than one solid. Copied from the **BaseApp/Preferences/Mod/PartDesign/AllowCompoundDefault** preference. |

---

## Advanced usage

### Retargeting a clone to a different source

Open the clone feature's *Properties* panel, locate `BaseFeature`, and click
the `...` button to select a new source object. The clone immediately
recomputes from the new source. Any features added on top of the clone in the
same Body are rebuilt against the new shape.

### Cross-document clones

Because `BaseFeature` uses `LinkScope::Global`, you can have a clone in one
document reference a solid in another document (when both are open in the same
FreeCAD session). This is useful for assembly-style workflows: master parts
live in their own documents, and each assembly document holds clones that track
them.

### Adding features on top of a clone

After activating the clone's Body, use any PartDesign tool normally. The clone
is automatically treated as the base solid for the next additive or subtractive
feature. The model tree will show the clone at the bottom, with your new
features stacked above it.

### Placement independence

The clone's `Placement` is separate from the source's `Placement`. You can
move the clone to a different position in the world while the geometry (shape)
remains synchronized with the source. This is unlike a simple instance or
link — the clone is an independent Body with its own location.

---

## Common mistakes and pitfalls

!!! warning "Clone command is greyed out"
    **Cause:** No object is selected, more than one object is selected, or the
    selected object is not derived from `Part::Feature`.  
    **Fix:** Select exactly one solid (a Body, a Part primitive, an imported
    solid, etc.) in the model tree or 3D viewport before activating the command.

!!! warning "Clone turns orange or shows an error after the source changes"
    **Cause:** The source object failed to recompute, so the clone has nothing
    valid to link to. Or the source was deleted.  
    **Fix:** Resolve the error in the source object first. If the source was
    deleted, either undo the deletion or retarget `BaseFeature` to a
    different object in the Properties panel.

!!! warning "Features added on top of the clone fail after source geometry changes"
    **Cause:** The underlying shape changed significantly (faces or edges were
    removed or renumbered) and PartDesign features that referenced those
    sub-shapes can no longer find them. This is the standard topological naming
    problem.  
    **Fix:** Re-attach the affected feature's sketch or references to the new
    geometry by editing the feature.

!!! warning "Circular dependency error"
    **Cause:** You tried to clone an object that is itself derived from (or
    depends on) the same Body you are editing.  
    **Fix:** The clone must reference an object that is independent of the
    clone's own Body. Use [Shape Binder](shape-binder.md) for referencing
    geometry from within the same document hierarchy.

---

## Python API

```python
import FreeCAD as App
import FreeCADGui as Gui

doc = App.newDocument()

# --- Create a source solid ---
# (Using a Part Box as the source; any Part::Feature works)
import Part
box = doc.addObject("Part::Box", "SourceBox")
box.Length = 50
box.Width = 30
box.Height = 20
doc.recompute()

# --- Create a Body and a FeatureBase (clone) ---
body_name = "Body"
clone_name = "Clone"

body = doc.addObject("PartDesign::Body", body_name)
clone = doc.addObject("PartDesign::FeatureBase", clone_name)

# Wire the body up: group contains the clone, tip points to it
body.Group = [clone]
body.Tip = clone

# Link the clone to the source
clone.BaseFeature = box
clone.Placement = box.Placement  # start at same position as source
clone.setEditorMode("Placement", 0)  # make Placement editable

doc.recompute()

print(clone.Shape.BoundBox)
# Expected: BoundBox identical to box.Shape.BoundBox

# --- Move the clone independently ---
import Base
clone.Placement = App.Placement(
    App.Vector(100, 0, 0),   # offset 100 mm along X
    App.Rotation()
)
doc.recompute()
# The clone shape is still the same box geometry, just translated.
```

### Key properties

| Property | Type | Description |
|----------|------|-------------|
| `BaseFeature` | `App::PropertyLink` | Points to the source `Part::Feature`. Setting this to `None` will cause the feature to error on next recompute. The link scope is Global. |
| `Placement` | `App::PropertyPlacement` | Position and orientation of the clone in the world coordinate system. Independent of the source's Placement. |
| `Shape` | `Part::PropertyPartShape` | Output shape, copied from `BaseFeature` on every recompute. Read-only. |

---

## See also

- [Shape Binder](shape-binder.md) — brings sub-shapes from another Body into
  the current Body as references
- [Sub-Shape Binder](sub-shape-binder.md) — more flexible binder that tracks
  relative placements and supports external documents
- [Duplicate Object](duplicate-selection.md) — produces a fully independent
  (unlinked) copy of the selected feature
- [Body](body.md) — the container that every PartDesign solid lives in
