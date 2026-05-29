# Snapping

> **In one sentence:** Snapping magnetises the cursor to geometric features
> of existing objects — endpoints, midpoints, centres, intersections, and
> more — so every point you place is precisely connected to the model rather
> than landing at an arbitrary location.

---

## What snapping does

When a Draft drawing tool is active and you move the cursor over the
3-D view, FreeCAD's snap engine tests each enabled snap type against nearby
geometry. The first matching snap is highlighted with a coloured marker.
Clicking places the point exactly at the snapped location — not at the raw
cursor pixel position.

Without snapping, lines and arcs almost never meet precisely: they appear
to connect visually but have tiny gaps that cause problems in downstream
operations (Part unions, offset paths, mesh export). Snapping eliminates
those gaps.

---

## The snap toolbar

All snap types are controlled from the **Draft Snap** toolbar. Each button
is an independent toggle — click to enable or disable that snap type.

### Master toggle: Snap Lock

**[Snap Lock](../tools/snap-lock.md)** (`Draft_Snap_Lock`, shortcut **S**
while a tool is active) is the master on/off switch. When Snap Lock is off,
all individual snap types are disabled regardless of their own toggle state.

!!! warning "Snap Lock is easy to toggle accidentally"
    If all snapping suddenly stops working, check the padlock icon in the
    Snap toolbar — **S** is easy to hit while typing in dimension dialogs.

---

## Snap types

### Point snaps

| Snap type | Indicator | What it snaps to |
|-----------|-----------|-----------------|
| [Endpoint](../tools/snap-endpoint.md) | Yellow square | Start/end vertices of lines, arcs, splines |
| [Midpoint](../tools/snap-midpoint.md) | Yellow triangle | Midpoint of any edge |
| [Center](../tools/snap-center.md) | Yellow circle cross-hair | Centre of a circle or arc; centroid of a face |
| [Angle](../tools/snap-angle.md) | Yellow dot on arc | 45° increment points on a circle (0°, 45°, 90°, …) |
| [Intersection](../tools/snap-intersection.md) | White cross | Crossing of two edges (real or virtual) |
| [Near](../tools/snap-near.md) | Yellow dot (sliding) | Nearest point on any edge — slides freely along the edge |
| [Special](../tools/snap-special.md) | Object-specific | Object-specific points (pole, knot, focus) |

### Direction / extension snaps

| Snap type | Indicator | What it snaps to |
|-----------|-----------|-----------------|
| [Extension](../tools/snap-extension.md) | Dashed yellow line | Along the infinite extension of a line beyond its endpoint |
| [Parallel](../tools/snap-parallel.md) | Dashed yellow line | Along a direction parallel to an existing edge |
| [Perpendicular](../tools/snap-perpendicular.md) | Yellow cross with ⊥ | Foot of the perpendicular from the cursor to an edge |
| [Ortho](../tools/snap-ortho.md) | Dashed yellow line | 45° grid directions from the last placed point |

### Grid and plane snaps

| Snap type | Indicator | What it snaps to |
|-----------|-----------|-----------------|
| [Grid](../tools/snap-grid.md) | Yellow cross | Nearest visible grid intersection |
| [Working Plane](../tools/snap-working-plane.md) | None (silent) | Projects any snap point down onto the working plane |

### Display aid

| Snap type | Indicator | What it does |
|-----------|-----------|-------------|
| [Dimensions](../tools/snap-dimensions.md) | Δx, Δy, Δz labels | Shows live distance labels next to the cursor — informational only, does not change snap behaviour |

---

## How the snap engine chooses

When multiple snap types could match the cursor position, the snap engine
applies a **priority order**:

1. **Endpoint** and **Midpoint** — strongest, always preferred when close enough.
2. **Center**, **Perpendicular**, **Near** — checked next.
3. **Intersection** — requires both edges to be close to the cursor.
4. **Grid** — lowest priority; only fires when nothing else matches.

The snap tolerance (in pixels) is set in
**Edit → Preferences → Draft → General → Snap range**.

---

## Snap and the working plane

The **Working Plane** snap type silently projects every other snap point
onto the current working plane. This ensures that even when you snap to
geometry that is above or below the working plane (e.g. a vertex at
Z = 50 mm while working on the XY plane at Z = 0), the placed point lands
on the working plane.

To place a point exactly on an object at a different Z level, **disable
the Working Plane snap** — then the snap fires at the 3-D location of the
geometry rather than its projection.

---

## Suppressing snapping temporarily

Hold **Shift** while clicking to suppress all snapping for a single click.
The cursor reverts to free positioning for that one point, then snapping
resumes automatically.

---

## Performance considerations

When working with very complex geometry (large assemblies, dense meshes):

- Disable **Near** and **Intersection** — these require the snap engine to
  test every edge in the scene for each cursor movement.
- Disable snaps you are not actively using.
- Use **Snap Lock** to turn off all snapping during rapid geometry placement
  and re-enable when precision is needed.

---

## Typical snap workflow

```
1. Set the working plane (select a face or named plane)
2. Enable Snap Lock (S)
3. Enable snap types: Endpoint + Midpoint (always on)
                      + Center (for arcs/circles)
                      + Grid (for floor plans)
4. Draw — the cursor snaps automatically
5. Hold Shift for a single free-placement point
6. Disable Near/Intersection if the scene is slow
```

---

## Python API

Snap behaviour is a UI feature — it is controlled interactively, not via
the Python API. The snap toolbar state is not exposed to scripting. To
place points programmatically, use explicit coordinates:

```python
import FreeCAD as App, Draft

# Place a line from explicit vertex to explicit vertex — no snapping needed
line = Draft.make_line(
    App.Vector(0, 0, 0),
    App.Vector(100, 50, 0)
)
App.ActiveDocument.recompute()
```

---

## See also

- [Snap Lock](../tools/snap-lock.md) — master snap toggle
- [Show Snap Bar](../tools/show-snap-bar.md) — restore the snap toolbar if closed
- [Select Working Plane](../tools/select-working-plane.md) — set the plane snapping projects onto
- [Toggle Grid](../tools/toggle-grid.md) — show or hide the grid
- [Draft Workbench](../index.md) — workbench overview
