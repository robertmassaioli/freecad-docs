# Shape Binder

> **In one sentence:** Copy geometry from another body or document object into
> the active Body so it can be used as a sketch support or feature reference.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Shape Binder  
**Toolbar:** none · **Shortcut:** none

Shape Binder creates a `PartDesign::ShapeBinder` object inside the active Body
that mirrors selected geometry from a *different* Part Design Body (or any
`Part::Feature`). The copied geometry can be a whole solid, a face, an edge, or
a vertex. Because the binder lives inside the Body, it can be used as a sketch
attachment surface, as a reference face for dimensions, or as an input to
profile-based features such as Pad or Additive Loft — even though the original
geometry belongs to a different Body.

---

## Intuition

Imagine tracing paper laid over a drawing. You can see and use the lines from
the sheet below without modifying the original. Shape Binder works the same
way: it gives you a read-only copy of geometry that "shines through" from
another Body.

The key reason this is necessary is Part Design's Body isolation rule: features
inside one Body cannot directly reference geometry inside a different Body. Shape
Binder acts as the bridge — it lives inside the target Body but holds a link to
the source, so the Body's features can reference it without breaking the
isolation rule.

When `TraceSupport` is enabled the binder's placement updates automatically as
the source object moves. This is useful for multi-Body assemblies where bodies
are repositioned relative to each other.

---

## When to use it

- You are building a second Body whose sketch should align with or snap to a
  face of the first Body.
- You want to use the outline of an existing solid (from another Body) as the
  profile for a Pad or Additive Loft.
- You need a planar reference face from another Body to act as the attachment
  plane for a new sketch.
- You are referencing a datum plane from the document root (Origin) that is
  outside the current Body.
- You need to bind to the complete shape of a body and optionally trace its
  placement.

---

## When NOT to use it

- **You need to reference individual sub-elements (faces, edges, vertices) from
  multiple objects at once** — use [Sub-Shape Binder](sub-shape-binder.md),
  which supports multi-object bindings, relative placement tracking, and
  configurable update modes.
- **You want to use geometry from within the same Body** — direct references to
  earlier features in the same Body are allowed without a binder.
- **You want the bound shape to remain independent after creation** — Sub-Shape
  Binder's *Detached* bind mode is better suited for that workflow.
- **The source geometry is in an external document and you need cross-document
  links** — Sub-Shape Binder's `Support` uses `PropertyXLinkSubList` which
  handles external documents; Shape Binder's `Support` is
  `PropertyLinkSubListGlobal` which also works across documents, but Sub-Shape
  Binder provides more control.

---

## Step-by-step walkthrough

**Prerequisites:** At least one Body must already contain a solid or other
geometry that you want to reference. The target Body (where the binder will
live) must be active.

1. **Activate the target Body.** Double-click it in the model tree. The active
   Body is shown in bold.

2. **Select the geometry to bind.** In the viewport or model tree, click the
   source object you want to reference. You can select a whole Body, a specific
   face, edge, or vertex by clicking the sub-element in the 3-D view.

3. **Invoke Shape Binder.** Go to Part Design → Shape Binder.

    - If a `ShapeBinder` object was selected when you invoked the command, the
      command opens that existing binder for editing instead of creating a new one.

4. **Adjust the reference in the task panel.** The panel shows an **Object**
   button and an **Add Geometry** / **Remove Geometry** toggle. Click **Add
   Geometry** and pick additional sub-elements if needed; click **Remove
   Geometry** to deselect. The list below shows the currently bound elements.

5. **Click OK.** The Shape Binder appears in the model tree under the active
   Body. It displays as a transparent shape in the viewport (its appearance
   differs from solid features).

6. **Use the binder.** You can now attach a new sketch to it (create a New
   Sketch and select the binder's face as the attachment support), or use it as
   an input to a profile-based feature.

!!! tip
    To enable placement tracking — so the binder updates when the source Body
    moves — set the `TraceSupport` property to `True` in the property panel
    after creating the binder.

---

## Parameters

The task panel has three controls:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Object | Button + text field | — | Activates selection mode for the base object. Shows the name of the currently linked object. |
| Add Geometry | Toggle button | — | When toggled on, clicking geometry in the viewport adds it to the reference list. |
| Remove Geometry | Toggle button | — | When toggled on, clicking a list entry removes it from the reference list. |
| References list | List | — | Displays the currently bound sub-elements (faces, edges, vertices). |

The following additional property is visible in the **property panel** after
creation:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Support` | `App::PropertyLinkSubListGlobal` | — | The source object and its sub-elements to copy. Setting this property programmatically is equivalent to using the task panel. Only one `Part::Feature` source is allowed at a time. |
| `TraceSupport` | `App::PropertyBool` | `False` | When `True`, the binder monitors placement changes on the source object (and its container Body/Part) and automatically recomputes its own placement to match. |

!!! note
    The `Placement` property of a Shape Binder is hidden in the property panel
    by default. It is set programmatically from the source shape's transform when
    the binder recomputes.

---

## Advanced usage

### Shape Binder as a sketch attachment plane

A Shape Binder that captures a single planar face appears in the plane-picker
dialog when you invoke New Sketch. This means you can bind a face from another
Body and then create a sketch aligned to that face — all without violating Body
isolation.

### Binding to Origin datum objects

Shape Binder can reference not only `Part::Feature` objects but also Origin
primitives such as `App::Plane`, `App::Line`, and `App::Point`. This is useful
when you need to anchor geometry in one Body relative to a datum from the
document's root coordinate system.

### Re-editing an existing Shape Binder

If you select a Shape Binder in the model tree and invoke Part Design → Shape
Binder again, the command opens the existing binder's task panel for editing
rather than creating a new one. This is the intended way to change the bound
geometry.

---

## Common mistakes and pitfalls

!!! warning "Binder is empty after recompute"
    **Cause:** The source feature was deleted or renamed, breaking the `Support`
    link.  
    **Fix:** Select the binder, open Part Design → Shape Binder to edit it, and
    re-assign the support to the new source geometry.

!!! warning "Cyclic dependency error when creating binder"
    **Cause:** The Body itself was selected as the support source. A Body cannot
    reference itself.  
    **Fix:** The command automatically removes the active Body from the selection.
    If the error persists, check that you are referencing geometry from a
    *different* Body, not the one the binder will live in.

!!! warning "Only one Part::Feature source is allowed"
    **Cause:** Shape Binder's `getFilteredReferences` filters to the first
    `Part::Feature` found in the selection; additional objects are silently
    ignored.  
    **Fix:** If you need geometry from multiple objects, use
    [Sub-Shape Binder](sub-shape-binder.md) instead, which supports
    multi-object bindings via `PropertyXLinkSubList`.

!!! warning "Shape is correct but placement is wrong in multi-body assembly"
    **Cause:** `TraceSupport` is `False` (the default) so the binder does not
    follow the source body when it is moved.  
    **Fix:** Set `TraceSupport = True` in the property panel.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# --- Body 1: a simple box ---
body1 = doc.addObject("PartDesign::Body", "Body")
box = doc.addObject("PartDesign::AdditiveBox", "Box")
box.Length = 20
box.Width = 15
box.Height = 10
body1.addObject(box)
doc.recompute()

# --- Body 2: a Shape Binder referencing Face1 of the box ---
body2 = doc.addObject("PartDesign::Body", "Body001")
binder = doc.addObject("PartDesign::ShapeBinder", "ShapeBinder")
binder.Support = [(box, "Face1")]   # bind the top face  # TODO: verify face index
body2.addObject(binder)
doc.recompute()

print(f"Binder shape area: {binder.Shape.Area:.2f} mm²")
```

### Key properties on the resulting feature object

| Property | Type | Description |
|----------|------|-------------|
| `Support` | `App::PropertyLinkSubListGlobal` | Source object and sub-element names. Setting `Support = [(obj, 'FaceN')]` binds a single face; `Support = [(obj, '')]` binds the whole shape. |
| `TraceSupport` | `App::PropertyBool` | Enable/disable automatic placement tracking when the source moves. |
| `Shape` | `Part::PropertyPartShape` | The resulting bound shape (read-only; computed on recompute). |

---

## See also

- [Sub-Shape Binder](sub-shape-binder.md) — more capable sibling: multiple sources, relative placement, configurable bind modes
- [New Sketch](new-sketch.md) — a Shape Binder face can serve as a sketch attachment surface
- [Clone](clone.md) — parametrically copy an entire solid into a new Body
