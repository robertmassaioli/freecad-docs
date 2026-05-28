# Utility Tools

> **In one sentence:** The Utilities menu provides tools for managing
> layers, groups, visual styles, the working plane, construction mode,
> and the snap toolbar — all the infrastructure that organises a Draft
> document.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Utilities

| Tool | Description |
|------|-------------|
| [Set Style](#set-style) | Apply a style preset to selected objects |
| [Apply Style](#apply-style) | Apply the current tool style to selected objects |
| [Layer](#layer) | Create a new layer |
| [Layer Manager](#layer-manager) | Open the layer manager dialog |
| [Add Named Group](#add-named-group) | Create a named group container |
| [Select Group](#select-group) | Select all objects in a group |
| [Toggle Construction Mode](#toggle-construction-mode) | Toggle construction geometry mode |
| [Add to Layer](#add-to-layer) | Assign selected objects to a layer |
| [Add to Group](#add-to-group) | Add selected objects to a group |
| [Add to Construction](#add-to-construction) | Move objects to the construction group |
| [Toggle Display Mode](#toggle-display-mode) | Switch Flat Lines ↔ Wireframe display |
| [Toggle Grid](#toggle-grid) | Show or hide the Draft grid |
| [Select Working Plane](#select-working-plane) | Set the active working plane |
| [Working Plane Proxy](#working-plane-proxy) | Save the current working plane |
| [Heal](#heal) | Repair malformed Draft objects |
| [Show Snap Bar](#show-snap-bar) | Bring back the hidden snap toolbar |

---

## Intuition

### Layers vs Groups

Draft uses two organizational concepts that serve different purposes:

| Concept | Purpose | Stored as |
|---------|---------|-----------|
| **Layer** | Visual styling (colour, line width, draw style) — all members share the layer's visual properties | `App::DocumentObjectGroup` with visual overrides |
| **Group** | Logical organisation — no visual effect | `App::DocumentObjectGroup` |

A layer is like the AutoCAD concept of a layer: assign objects to it, and
changing the layer's colour changes all members at once. A group is purely
for organization in the model tree.

Objects can belong to both a layer (for style) and a group (for logic).

### Construction mode

Construction mode adds all subsequently-created Draft objects to a special
"Construction" layer coloured in a distinct colour (usually blue). These
objects serve as geometric references and are normally hidden before export.

---

## Set Style

**Menu:** Draft → Utilities → Set Style  
**Command:** `Draft_SetStyle`

Opens the style task panel to set the **current tool style** — the default
visual properties applied to all newly-created Draft objects. This does
not affect existing objects.

### Style properties

| Property | Description |
|----------|-------------|
| Line color | RGB colour for lines and edges |
| Line width | Display line width (pixels) |
| Shape color | Fill colour for faces |
| Shape transparency | 0–100% opacity |
| Text color | Colour for Text and Dimension text |
| Text size | Default text height |
| Font name | Default font |
| Arrow size | Size of dimension and label arrows |
| Arrow type | Dot, Arrow, Tick, Open Arrow, etc. |

Styles can be saved as named presets (see Annotation Style Editor).

---

## Apply Style

**Menu:** Draft → Utilities → Apply Style  
**Command:** `Draft_ApplyStyle`

Applies the **current tool style** (from Set Style) to selected objects.
Use after changing the tool style to update existing objects to match.

---

## Layer

**Menu:** Draft → Utilities → Layer  
**Command:** `Draft_Layer`

Creates a new layer in the document. Layers appear in the model tree
under a "Layers" container. Each layer has:

| Property | Description |
|----------|-------------|
| Line Color | Line colour for all layer members |
| Line Width | Line width for all layer members |
| Draw Style | Solid, Dashed, Dotted, DashDot |
| Shape Color | Fill colour for layer member faces |
| Transparency | Transparency for layer member faces |
| Visibility | Show/hide all objects on the layer at once |

### Python API

```python
import Draft

layer = Draft.make_layer(
    name="Walls",
    line_color=(0.0, 0.0, 0.0, 1.0),   # RGBA black
    shape_color=(0.8, 0.8, 0.8, 1.0),
    line_width=1.0,
    draw_style="Solid",
    transparency=0
)
App.ActiveDocument.recompute()
```

---

## Layer Manager

**Menu:** Draft → Utilities → Layer Manager  
**Command:** `Draft_LayerManager`

Opens the Layer Manager panel, which shows all layers in a table with
columns for visibility, locking, colour, line width, and draw style.
From here you can:

- Toggle the visibility of each layer.
- Add, rename, or delete layers.
- Change any layer property for all members at once.
- Merge one layer into another.
- Select all objects on a layer.

The Layer Manager is the primary interface for managing layers in a
Draft document.

---

## Add Named Group

**Menu:** Draft → Utilities → Add Named Group  
**Command:** `Draft_AddNamedGroup`

Prompts for a group name and creates a new `App::DocumentObjectGroup`
in the model tree with that name. Equivalent to creating a group in
the Part workbench but accessible directly in Draft.

---

## Select Group

**Menu:** Draft → Utilities → Select Group  
**Command:** `Draft_SelectGroup`

Selects all objects in the same group or layer as the currently selected
object. Useful for quickly selecting all geometry on a layer before
moving it or changing its style.

---

## Toggle Construction Mode

**Menu:** Draft → Utilities → Toggle Construction Mode  
**Command:** `Draft_ToggleConstructionMode`

Turns construction geometry mode on or off. When active:

- A green indicator appears in the Draft toolbar.
- All newly-created Draft objects are added to the "Construction" group.
- Objects receive the construction colour (default: blue).

Construction objects are typically hidden before export and serve only
as references during modelling.

---

## Add to Layer

**Menu:** Draft → Utilities → Add to Layer  
**Command:** `Draft_AddToLayer`

Moves selected objects to a chosen layer. A submenu lists all existing
layers. Select one or more objects, then pick the target layer.

---

## Add to Group

**Menu:** Draft → Utilities → Add to Group  
**Command:** `Draft_AddToGroup`

Adds selected objects to an existing group (without visual style change).
A submenu lists all existing groups.

---

## Add to Construction

**Menu:** Draft → Utilities → Add to Construction  
**Command:** `Draft_AddConstruction`

Moves selected objects to the Construction group and applies the
construction colour. The inverse of removing an object from construction
mode.

---

## Toggle Display Mode

**Menu:** Draft → Utilities → Toggle Display Mode  
**Command:** `Draft_ToggleDisplayMode`

Switches selected Draft objects between the two display modes:

| Mode | Description |
|------|-------------|
| Flat Lines | Show filled faces with edge lines (default) |
| Wireframe | Show edges only — no face fill |

Useful for quickly making overlapping faces transparent to see geometry
underneath.

---

## Toggle Grid

**Menu:** Draft → Utilities → Toggle Grid  
**Command:** `Draft_ToggleGrid`

Shows or hides the Draft grid in the 3-D view. The grid is a visual
reference plane at the current working plane; it does not constrain
snapping (the Snap Grid snap type handles grid snapping separately).

### Grid settings

Configure the grid in **Edit → Preferences → Draft → Grid**:

| Setting | Description |
|---------|-------------|
| Grid spacing | Distance between grid lines (mm) |
| Main lines every N | Bold grid lines at every Nth interval |
| Grid size | Number of grid squares visible |
| Grid color | Colour of grid lines |

---

## Select Working Plane

**Menu:** Draft → Utilities → Select Working Plane  
**Command:** `Draft_SelectPlane`

Sets the active working plane — the plane on which all Draft geometry
is placed. Options:

| Preset | Normal vector |
|--------|--------------|
| Top (XY) | +Z |
| Front (XZ) | +Y |
| Side (YZ) | +X |
| Custom | From selected face or 3 points |
| Working Plane Proxy | Restored from a saved proxy |

### Setting from a face

1. Select a face of an existing solid.
2. Activate **Select Working Plane**.
3. The working plane snaps to that face (co-planar with the face).

All subsequent Draft geometry is placed on this plane.

### Setting from 3 points

Activate **Select Working Plane** with nothing selected, then click three
points in the 3-D view to define the plane (three non-collinear points
define a unique plane).

### Python API

```python
import WorkingPlane

# Set to XY plane at Z=0
wp = WorkingPlane.get_working_plane()
wp.align_to_point_and_axis(
    App.Vector(0, 0, 0),
    App.Vector(0, 0, 1),
    0
)
```

---

## Working Plane Proxy

**Menu:** Draft → Utilities → Working Plane Proxy  
**Command:** `Draft_WorkingPlaneProxy`

Saves the current working plane as a **proxy object** in the model tree.
Double-clicking the proxy in the model tree restores that working plane.

Proxies are useful for documents with multiple views from different
working planes — save each plane as a proxy and switch between them
by double-clicking.

---

## Heal

**Menu:** Draft → Utilities → Heal  
**Command:** `Draft_Heal`

Attempts to repair Draft objects that have become malformed — typically
objects created in older FreeCAD versions with deprecated internal
properties. Heal upgrades them to the current Draft object format.

If geometry appears missing or a Draft object throws errors in the model
tree, try running Heal on it.

---

## Show Snap Bar

**Menu:** Draft → Utilities → Show Snap Bar  
**Command:** `Draft_ShowSnapBar`

Re-displays the Draft Snap toolbar if it has been closed. The snap
toolbar can be accidentally closed; this command restores it.

---

## Common mistakes and pitfalls

!!! warning "Layer colour overrides object colour"
    When an object belongs to a layer, the layer's colour properties
    take precedence over the object's own colour properties. If you
    change an object's colour in its Properties panel and nothing
    changes, check whether the object is on a layer.

!!! warning "Construction objects are still saved in the document"
    Toggling off construction mode does not delete construction objects —
    they remain in the model tree. To hide them, toggle the visibility
    of the Construction group. To delete them, select and press Delete.

!!! warning "Working plane resets after certain operations"
    Some operations (attaching to a face, switching workbenches) may
    reset the working plane to the default XY plane. Use a Working Plane
    Proxy to reliably restore a non-standard working plane.

!!! warning "Groups and layers are independent"
    Adding an object to a group does not add it to a layer, and vice versa.
    An object can be in a group (for organisation) and on a layer (for style)
    simultaneously. Manage both separately via Add to Group and Add to Layer.

---

## See also

- [Drafting Tools](drafting.md) — geometry that goes on layers
- [Snapping](snapping.md) — snap toolbar and snap types
- [Annotation Tools](annotation.md) — annotation style management
