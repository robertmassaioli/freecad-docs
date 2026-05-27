# View Tools (View Sketch, View Section, Grid, Snap, Rendering Order)

> **In one sentence:** A set of display helpers that control how a sketch and its
> environment are shown while you are in sketch edit mode — they affect only the
> view, never the geometry or constraints.

---

## Overview

**Workbench:** Sketcher · **Menu:** Sketch → View / Sketch Display  
**Toolbar:** Sketcher view toolbar · **Shortcut:** see table

These tools do not create or modify sketch geometry. They exist to make it easier
to see what you are doing while editing.

| Tool | Shortcut | What it does |
|------|----------|-------------|
| View Sketch | none | Re-centre and orient the camera so the sketch plane fills the view |
| View Section | none | Toggle a cutting plane that clips geometry above the sketch plane so solid faces do not obscure the sketch |
| Grid | none | Toggle the background grid and set its spacing |
| Snap | none | Toggle snapping of the cursor to grid intersections and special geometry points |
| Rendering Order | none | Control the draw order of sketch elements relative to 3-D geometry |

---

## Intuition

These tools exist because sketching in 3-D space has a visibility problem: the
solid your sketch is attached to can block the sketch edges, the camera angle
can make the sketch look like an ellipse instead of a circle, and freehand
cursor placement is imprecise without grid guidance.

None of these tools touch geometry or constraints. They are pure view aids:

- **View Sketch** snaps you back to the face-on camera angle whenever you
  accidentally orbit the model and lose the 2-D perspective.
- **View Section** clips away the solid above the sketch plane so thick faces
  cannot hide what you are drawing.
- **Grid / Snap** bring drafting-table precision — snap points align to a
  configurable pitch so short distances are easy to enter without typing.
- **Rendering Order** resolves z-fighting when sketch lines sit exactly on a
  solid face and flicker between foreground and background.

Think of this group as your sketch workspace setup: configure it once at the
start of a sketching session and forget about it.

---

## View Sketch

**Menu:** Sketch → View Sketch

Rotates and zooms the camera so that it looks directly down the sketch's normal
axis — i.e. you see the sketch face-on, exactly as it appears in 2-D. Use this
whenever the camera has drifted from the sketch plane.

### When to use it

- You tumbled the 3-D view and lost track of the sketch orientation.
- You want to take a straight-on measurement in the viewport.
- You are about to select overlapping geometry and need a face-on view to pick
  correctly.

---

## View Section

**Menu:** Sketch → View Section

Toggles a clipping plane aligned with the sketch. When active, geometry *above*
the sketch plane (on the far side from the camera) is hidden, leaving only the
sketch layer and geometry behind it visible. This prevents solid faces from
obscuring fine sketch geometry on interior faces.

### When to use it

- You are sketching on a recessed face deep inside a solid and the outer walls
  block the view.
- You need to see construction geometry on a face that a solid overlaps.

---

## Grid

**Menu:** Sketch → Grid

Toggles the sketch grid overlay and opens a settings panel where you can configure:

| Setting | Description |
|---------|-------------|
| Grid auto spacing | Automatically adjusts grid density as you zoom |
| Grid spacing | Manual grid cell size in mm when auto-spacing is off |
| Grid lines | Number of lines / divisions |

The grid is visual only — it does not constrain geometry unless **Snap to Grid** is
also enabled.

---

## Snap

**Menu:** Sketch → Snap

Toggles cursor snapping behaviour. When enabled, the cursor snaps to:

- Grid intersections (if the grid is visible and Snap to Grid is on)
- Endpoints and midpoints of existing geometry
- The sketch origin
- Intersection points of visible edges

Snapping is useful for rough geometry placement but should not replace formal
constraints — never rely on a "snapped" vertex instead of a Coincident constraint,
because snapped positions are approximate.

!!! warning "Snap is not a substitute for constraints"
    Vertices that appear to be coincident because they snapped together are often
    separate points. Use [Validate Sketch](validate-sketch.md) to detect these gaps,
    or explicitly apply a [Coincident](constraint-coincident.md) constraint.

---

## Rendering Order

**Menu:** Sketch → Rendering Order

Controls whether sketch elements are drawn on top of or behind the 3-D solid
geometry in the viewport. Useful when geometry elements are visually obscured by
solid faces even when View Section is off.

---

## Python API

View tools affect the Gui layer only and are not exposed via the Python scripting
API. They have no effect in headless / console mode.

---

## See also

- [New Sketch / Edit Sketch](new-sketch.md) — entering and leaving sketch edit mode
- [Validate Sketch](validate-sketch.md) — detecting and fixing false near-coincident vertices caused by snap
- [Coincident](constraint-coincident.md) — the correct way to join two vertices
