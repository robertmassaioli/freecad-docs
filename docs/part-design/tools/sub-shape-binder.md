# Sub-Shape Binder

> **In one sentence:** Reference geometry from one or more objects — entire
> solids, faces, edges, or vertices — inside the active Body, with automatic
> placement tracking and configurable update behaviour.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Sub-Shape Binder  
**Toolbar:** Part Design Helper Features · **Shortcut:** none

Sub-Shape Binder creates a `PartDesign::SubShapeBinder` object that captures
geometry from one or more source objects and makes it available inside the
active Body (or as a standalone document object if no Body is active). Unlike
[Shape Binder](shape-binder.md), it can reference sub-elements from *multiple*
sources simultaneously, tracks relative placement changes between the binder and
its sources, supports cross-document references, and offers three update modes
that let you control when (or whether) the bound shape refreshes.

---

## Intuition

Shape Binder is like tracing paper — you lay it over one sheet and trace. Sub-Shape
Binder is like a smart copy-and-paste that can pull from several sheets at once,
remember where each piece came from, and optionally keep itself in sync as the
originals change.

The key difference from Shape Binder is the *relative* placement model. When
`Relative = True` (the default), the binder stores the geometric relationship
between itself and its sources at creation time. If the source Body is later
moved — say you assemble bodies by repositioning them — the bound shape moves
with it automatically, without any manual adjustment. This makes Sub-Shape Binder
the right tool for multi-Body assembly workflows.

Sub-Shape Binder also supports an *offset* capability: you can expand or shrink
a bound 2-D wire or face by a configurable distance, which is useful for
generating clearance contours or wall profiles derived from existing geometry.

---

## When to use it

- You need to reference faces or edges from *multiple* different objects at once
  (Sub-Shape Binder accepts a mixed selection; Shape Binder only accepts one
  `Part::Feature`).
- You are building a multi-Body model and want the bound shape to follow the
  source Body when you move it (relative placement tracking).
- You want to temporarily freeze a snapshot of some geometry and prevent it from
  updating when the source changes (`BindMode = Frozen`).
- You need a one-time, permanently detached copy of geometry that has no further
  link to the source (`BindMode = Detached`).
- You need to reference geometry in an *external document* (cross-document
  `PropertyXLinkSubList` links).
- You want to offset a bound wire or face to derive a clearance profile
  (`Offset` property).

---

## When NOT to use it

- **You only need to bind to a single solid from one Body and do not need
  relative placement tracking** — [Shape Binder](shape-binder.md) is simpler
  and sufficient.
- **You want to copy an entire solid into a new Body as a parametric base
  feature** — use [Clone](clone.md), which creates a proper `PartDesign::Body`
  wrapper and sets `BaseFeature` automatically.
- **The source geometry is in the same Body** — direct references to earlier
  features in the same Body do not require a binder.

---

## Step-by-step walkthrough

**Prerequisites:** An open document. The target Body (if any) should be active.
Source geometry can be in any Body or standalone object.

1. **Select the geometry to bind.** In the viewport or model tree, select one or
   more objects, faces, edges, or vertices. Sub-Shape Binder captures the
   selection at the time you invoke it.

    !!! tip
        You can select sub-elements from different objects in one selection sweep:
        hold Ctrl and click each face or edge. All selected sub-elements become
        the binder's initial `Support`.

2. **Invoke Sub-Shape Binder.** Go to Part Design → Sub-Shape Binder, or click
   the Sub-Shape Binder icon in the "Part Design Helper Features" toolbar.

3. **The binder is created immediately.** There is no task-panel dialog. The
   `PartDesign::SubShapeBinder` object appears in the model tree (named "Binder"
   by default) and the bound shape is visible in the viewport.

4. **Configure properties if needed.** Open the property panel (View → Panels
   → Properties) and adjust:
   - `BindMode` — switch to `Frozen` if you want to lock the current shape, or
     `Detached` to cut the link entirely.
   - `Relative` — disable if you do *not* want placement-relative tracking.
   - `Offset` — set a positive or negative value to expand or shrink a bound
     2-D wire or face.

5. **Use the binder.** Attach a sketch to it via New Sketch, or reference the
   bound face in a feature such as Pad or Revolution.

---

## Parameters

Sub-Shape Binder has no task panel. All configuration is done through the
**property panel** after creation.

### Base group

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Support` | `App::PropertyXLinkSubList` | — | Source objects and their sub-elements. Accepts multiple objects. Set programmatically or by re-running the command with a new selection. |
| `BindMode` | Enumeration | `Synchronized` | Update policy for the bound shape. See deep dive below. |
| `Relative` | `App::PropertyBool` | `True` | When `True`, placement is stored relative to the source's container Body. When `False`, placement is in absolute document coordinates. |
| `ClaimChildren` | `App::PropertyBool` | `False` | If `True`, the bound objects appear as children of this binder in the model tree. |
| `PartialLoad` | `App::PropertyBool` | `False` | When `True`, the binder does not force-load an external document containing the source. Useful for large assemblies. |
| `BindCopyOnChange` | Enumeration | `Disabled` | Parametric variation mode. See deep dive below. |
| `Refine` | `App::PropertyBool` | `True` | Remove redundant edges from the resulting shape after binding. |
| `Fuse` | `App::PropertyBool` | `False` | Fuse all bound solid sub-shapes into a single solid. |
| `MakeFace` | `App::PropertyBool` | `True` | Automatically create a face from bound wires when the result is otherwise a set of wires. |

### BindMode in depth

BindMode is the most important day-to-day property of a Sub-Shape Binder. It
controls when — or whether — the binder pulls updated geometry from its source.

#### Synchronized (default)

The binder recomputes every time the source feature changes. The bound shape
always matches the current state of the source. This is the "live link" mode.

Intuition: the binder is a mirror — any change to the source is instantly
reflected in the binder's shape.

**Use this mode when:**
- You want the reference geometry in the current Body to automatically track
  changes in the source Body (the standard use-case for cross-Body references)
- The source feature is still being actively designed and you want the downstream
  geometry to update continuously

**Watch out for:** Every recompute of the source triggers a recompute of the
binder and everything downstream of it. In large models with many Synchronized
binders, this can slow recompute significantly. Use Frozen for references that
are stable and unlikely to change.

#### Frozen

Auto-update is disabled. The binder retains the shape it captured at the last
manual refresh and does not recompute when the source changes. To pull in
changes, right-click the binder in the model tree and choose **Refresh**.

Intuition: a photograph of the source taken at a moment in time — it does not
change until you choose to take a new photograph.

**Use this mode when:**
- The source geometry is finalised and should not affect the current Body's
  downstream features until explicitly approved
- Performance is a concern: freezing stable references prevents unnecessary
  recompute cascades
- You are implementing a design-intent approval workflow where cross-Body
  references are updated only at explicit checkpoints

**Watch out for:** It is easy to forget a Frozen binder and work with stale
geometry. Add a `<!-- TODO: refresh binder -->` comment in the model notes, or
use a naming convention (e.g. prefix the feature name with "FROZEN-") to make
frozen binders visible in the tree.

#### Detached

The parametric link to the source is permanently severed. The binder retains
the last-captured shape as static geometry. The `Support` property is cleared.

Intuition: the photograph was printed, the negative destroyed — the image is
permanent and independent of the original scene.

**Use this mode when:**
- You want a permanent copy of the geometry that survives even if the source
  feature is deleted or the source file is moved
- Exporting a snapshot of a cross-Body reference for use in a long-term archive

**Watch out for:** Detach is irreversible via the UI — you cannot re-attach to
the source without deleting the binder and creating a new one. The shape is
frozen permanently, not just until the next Refresh.

### BindCopyOnChange in depth

BindCopyOnChange enables a parametric variation workflow: the binder can hold a
locally modified copy of the source with different parameter values, producing a
different shape.

#### Disabled (default)

No copy-on-change behaviour. The binder simply tracks the source shape as-is.
This is correct for the vast majority of binders.

**Use this mode when:** You want a reference copy of the source geometry, not a
variant of it.

**Watch out for:** Nothing — Disabled is the safe default.

#### Enabled

When enabled, FreeCAD looks for properties on the source that are marked
`CopyOnChange`. If any exist, the binder creates a local independent copy of
the source feature and exposes those properties for editing on the binder. The
binder's shape is then computed from the local copy with its modified property
values.

Intuition: the binder becomes a "variant" of the source — same topology,
different dimensions. Like copying a parametric sketch and adjusting the
dimensions of the copy without affecting the original.

**Use this mode when:**
- You need multiple variants of a source feature with different parameter values
  in the same document (e.g. a family of similar parts)
- The source feature's author has explicitly marked certain properties as
  `CopyOnChange` to allow this workflow

**Watch out for:** Only properties explicitly tagged `CopyOnChange` by the
feature author are exposed for local editing. If no such properties exist,
Enabled behaves identically to Disabled. This is an advanced workflow that
requires deliberate setup on the source feature side.

#### Mutated

This value is set automatically by FreeCAD — you do not set it manually. It
means the binder has already been modified from the source via the copy-on-change
mechanism and has diverged. The binder now has its own independent copy.

**Use this mode when:** You do not set this manually; it is a status indicator.

**Watch out for:** Once Mutated, the binder no longer updates from the source
even if the source changes non-CopyOnChange properties. The link is partially
severed.

### Offsetting group

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Offset` | `App::PropertyFloat` | `0.0` | Expand (`>0`) or shrink (`<0`) the bound 2-D wire or face by this distance in mm. A value of `0.0` disables offsetting. |
| `OffsetJoinType` | Enumeration | `Arcs` | How corners are handled during offsetting: `Arcs` (default), `Tangent`, or `Intersection`. |
| `OffsetFill` | `App::PropertyBool` | `False` | When `True`, create a face spanning the space between the original wire and the offset wire. |
| `OffsetOpenResult` | `App::PropertyBool` | `False` | When `False`, open wires are closed before offsetting. Set `True` to keep the result open. |
| `OffsetIntersection` | `App::PropertyBool` | `False` | When `False`, child wires are offset independently. Set `True` to compute intersections between child wires during offsetting. |

---

## Advanced usage

### Relative vs. absolute placement

With `Relative = True` (default), the binder records its position *relative to
its source's container* at creation time. If the source Body is later moved —
for example in an assembly — the binder moves with it, maintaining the same
relative offset. With `Relative = False` the binder position is fixed in world
coordinates and does not follow source movement.

### Frozen and Detached modes

Use `Frozen` when you want to capture a snapshot of the geometry for reference
without being affected by ongoing changes to the source. Right-click the binder
in the model tree and choose **Refresh** to pull in an update manually.

Use `Detached` when you want a permanent copy that survives even if the source
feature is deleted. The binder's `Support` is cleared and only the shape data is
kept.

### Offsetting for clearance profiles

Set `Offset = 0.5` (for a 0.5 mm outward expansion) to derive a clearance
profile from an existing face. Combined with `OffsetJoinType = Arcs`, this
produces a smooth expanded outline suitable for generating a seal groove or
mounting hole pattern.

### Cross-document references

Sub-Shape Binder uses `PropertyXLinkSubList`, which supports cross-document
links. You can reference geometry in a separate `.FCStd` file. When
`PartialLoad = True` the external document is not loaded on startup, which
reduces memory use for large assemblies.

---

## Common mistakes and pitfalls

!!! warning "Binder does not update when source changes"
    **Cause:** `BindMode` was set to `Frozen` or `Detached`, disabling automatic
    updates.  
    **Fix:** Set `BindMode` back to `Synchronized`, or right-click the binder
    and choose **Refresh** if you intend to keep Frozen mode.

!!! warning "Binder shape is in the wrong position after moving a Body"
    **Cause:** `Relative = False`, so the binder position is fixed in world
    space rather than tracking the source.  
    **Fix:** Delete the binder and recreate it with `Relative = True` (the
    default), or manually adjust the binder's placement after each source move.

!!! warning "Unexpected face from wires"
    **Cause:** `MakeFace = True` (default) automatically builds a face from bound
    wires. This is usually helpful but can produce unexpected geometry if the
    wires are not planar or do not form a simple closed contour.  
    **Fix:** Set `MakeFace = False` if you want to keep the raw wires without
    face-filling.

!!! warning "Offset produces self-intersecting result"
    **Cause:** The offset distance is larger than the minimum radius of curvature
    in the source wire, causing the offset wire to self-intersect.  
    **Fix:** Reduce the `Offset` value, or switch `OffsetJoinType` to
    `Intersection` to handle concave corners differently.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# --- Body 1: a box ---
body1 = doc.addObject("PartDesign::Body", "Body")
box = doc.addObject("PartDesign::AdditiveBox", "Box")
box.Length = 30
box.Width = 20
box.Height = 15
body1.addObject(box)
doc.recompute()

# --- Body 2: a Sub-Shape Binder referencing the top face of the box ---
body2 = doc.addObject("PartDesign::Body", "Body001")
binder = body2.newObject("PartDesign::SubShapeBinder", "Binder")
# Bind to the top face of the box  # TODO: verify face index for this geometry
binder.Support = [(box, ("Face6",))]
doc.recompute()

print(f"Binder shape area: {binder.Shape.Area:.2f} mm²")
```

### Binding multiple sub-elements from different objects

```python
# Bind edges from two different features  # TODO: verify
binder.Support = [
    (box, ("Edge1", "Edge2")),
    (other_feature, ("Face1",)),
]
doc.recompute()
```

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `Support` | `App::PropertyXLinkSubList` | Source objects and sub-element name lists. |
| `BindMode` | `App::PropertyEnumeration` | Update mode: `"Synchronized"`, `"Frozen"`, or `"Detached"`. |
| `Relative` | `App::PropertyBool` | Enable relative placement tracking. |
| `Fuse` | `App::PropertyBool` | Fuse bound solids into one. |
| `MakeFace` | `App::PropertyBool` | Build a face from bound wires. |
| `Offset` | `App::PropertyFloat` | 2-D offset distance in mm. |
| `OffsetJoinType` | `App::PropertyEnumeration` | Corner join type for offsetting: `"Arcs"`, `"Tangent"`, `"Intersection"`. |
| `Refine` | `App::PropertyBool` | Remove redundant edges after binding. |
| `ClaimChildren` | `App::PropertyBool` | Show bound objects as children in the model tree. |
| `PartialLoad` | `App::PropertyBool` | Suppress auto-loading of external documents. |
| `BindCopyOnChange` | `App::PropertyEnumeration` | Copy-on-change mode: `"Disabled"`, `"Enabled"`, `"Mutated"`. |

---

## See also

- [Shape Binder](shape-binder.md) — simpler single-source binder; use when one Part::Feature source is sufficient
- [Clone](clone.md) — copy an entire solid into a new Body as a parametric base feature
- [New Sketch](new-sketch.md) — a Sub-Shape Binder face can serve as a sketch attachment surface
