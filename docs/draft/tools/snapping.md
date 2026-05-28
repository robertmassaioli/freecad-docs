# Snapping

> **In one sentence:** Draft's snapping system finds and locks the cursor
> to specific geometric features of existing objects — endpoints, midpoints,
> centres, intersections, perpendiculars, and more — so you can draw with
> precision without typing every coordinate.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft Snap toolbar (no menu — toolbar only)

| Snap type | Command | Description |
|-----------|---------|-------------|
| [Snap Lock](#snap-lock) | `Draft_Snap_Lock` | Master on/off toggle |
| [Endpoint](#endpoint) | `Draft_Snap_Endpoint` | Edge endpoints |
| [Midpoint](#midpoint) | `Draft_Snap_Midpoint` | Edge midpoints |
| [Center](#center) | `Draft_Snap_Center` | Circle/arc centres, face centres |
| [Angle](#angle) | `Draft_Snap_Angle` | 45° increment points on arcs |
| [Intersection](#intersection) | `Draft_Snap_Intersection` | Edge-edge intersections |
| [Perpendicular](#perpendicular) | `Draft_Snap_Perpendicular` | Perpendicular foot on an edge |
| [Extension](#extension) | `Draft_Snap_Extension` | Extension of an edge beyond its endpoint |
| [Parallel](#parallel) | `Draft_Snap_Parallel` | Direction parallel to an edge |
| [Special](#special) | `Draft_Snap_Special` | Object-specific special points |
| [Near](#near) | `Draft_Snap_Near` | Nearest point on an edge |
| [Ortho](#ortho) | `Draft_Snap_Ortho` | 45° intervals from the last point |
| [Grid](#grid) | `Draft_Snap_Grid` | Grid intersections |
| [Working Plane](#working-plane) | `Draft_Snap_WorkingPlane` | Project onto working plane |
| [Dimensions](#dimensions) | `Draft_Snap_Dimensions` | Live dimension display |
| [Toggle Grid](#toggle-grid-snap) | `Draft_ToggleGrid` | Show/hide the grid |

---

## How snapping works

When a Draft drawing tool is active and you move the cursor over the
3-D view:

1. FreeCAD tests each enabled snap type against nearby geometry.
2. The first matching snap is highlighted (a coloured marker appears).
3. Clicking places the point at the snapped position.
4. If multiple snaps compete, the one with the highest priority wins.

The **snap indicator** (coloured dot or cross) shows which snap type
is active. Its colour matches the snap type's icon colour.

### Snap priority

When multiple snaps overlap:

1. More specific snaps (Endpoint, Center) take priority over approximate
   snaps (Near, Grid).
2. Snaps on closer objects take priority over snaps on distant objects.
3. You can hold **Shift** to temporarily disable snapping and place a
   free point.

---

## Snap Lock

**Command:** `Draft_Snap_Lock`  
**Shortcut:** `S` (while a drawing tool is active)

The master toggle for all snapping. When Snap Lock is **off**, all snap
types are disabled and the cursor moves freely. This is equivalent to
disabling every individual snap at once.

Toggle Snap Lock with:
- The padlock icon in the snap toolbar.
- The `S` shortcut while any drawing tool is active.

---

## Endpoint

**Command:** `Draft_Snap_Endpoint`

Snaps to the endpoints of edges and wires — the vertices at the start
and end of line segments, arc endpoints, and spline ends.

**Indicator:** Yellow square at the endpoint.

Most-used snap type. Enable it for all general drafting.

---

## Midpoint

**Command:** `Draft_Snap_Midpoint`

Snaps to the midpoint of any edge — exactly halfway along the edge.

**Indicator:** Yellow triangle at the midpoint.

Useful for centring objects, placing reference lines at bisectors, and
aligning to the middle of walls or members.

---

## Center

**Command:** `Draft_Snap_Center`

Snaps to:
- The centre of a circle or arc.
- The centre of a sphere, cylinder, or cone face.
- The centroid of a planar face.
- The midpoint of an edge or wire (when no other centre applies).

**Indicator:** Yellow circle cross-hair.

---

## Angle

**Command:** `Draft_Snap_Angle`

Snaps to points at 45° increments around a circle or arc (0°, 45°, 90°,
135°, 180°, 225°, 270°, 315°). These are the quarter-points and
eighth-points of the circle.

**Indicator:** Yellow dot on the arc at 45° positions.

Useful for placing geometry at exact cardinal and diagonal positions on
circles.

---

## Intersection

**Command:** `Draft_Snap_Intersection`

Snaps to the intersection of two edges. Move the cursor near an
intersection point — FreeCAD computes the crossing of the two edges
(even if they do not physically touch, as in an extended intersection).

**Indicator:** White cross at the intersection.

!!! note "Virtual intersections"
    Intersection snap can find the intersection of edge extensions — the
    point where two edges would meet if extended. Move the cursor over one
    edge, then move toward the intersection area while keeping the first
    edge highlighted.

---

## Perpendicular

**Command:** `Draft_Snap_Perpendicular`

Snaps to the point on an edge that is perpendicular to the previous point
in the drawing sequence. This ensures the new segment is at 90° to the
target edge from the previous vertex.

**Indicator:** Yellow cross with perpendicularity symbol.

Useful for drawing lines that must be exactly normal to an existing edge.

---

## Extension

**Command:** `Draft_Snap_Extension`

Snaps along the extension of an existing edge — beyond the edge's
endpoint in the same direction. Move the cursor near an edge to lock in
the extension direction, then move along it.

**Indicator:** Dashed yellow line showing the extension direction.

Useful for placing objects in line with existing geometry but offset
beyond the existing endpoints.

---

## Parallel

**Command:** `Draft_Snap_Parallel`

Snaps along a direction **parallel** to an existing edge. Move the cursor
near an edge to lock in its direction, then move the cursor anywhere to
snap on lines parallel to that edge.

**Indicator:** Dashed yellow line showing the parallel direction.

Combination of Parallel + Extension is very powerful: snap along the
extension of one edge at a position parallel to another.

---

## Special

**Command:** `Draft_Snap_Special`

Snaps to object-specific special points that are not covered by other
snap types:

| Object | Special points |
|--------|---------------|
| Part Design Body | Origin, placement point |
| Arch Wall | Corner points, opening positions |
| BIM elements | Structural insertion points |
| Draft ShapeString | Character insertion points |

---

## Near

**Command:** `Draft_Snap_Near`

Snaps to the **nearest** point on any edge, regardless of whether it is
an endpoint, midpoint, or anywhere along the edge. This is the least
selective snap — it finds the closest position on any edge.

**Indicator:** Yellow dot sliding along the edge.

!!! note "Use Near with care"
    Near snap tends to override more specific snaps. Disable it when you
    want to pick specific features (endpoints, midpoints) and only enable
    it when you need to attach to an arbitrary edge position.

---

## Ortho

**Command:** `Draft_Snap_Ortho`

Snaps the cursor to positions at 45° intervals from the **previous
point** in the drawing sequence — 0°, 45°, 90°, 135°, 180°, 225°,
270°, 315°.

**Indicator:** Dashed yellow line at the ortho direction from the
last point.

When Ortho snap is active and the cursor is near an ortho direction, it
locks to that angle. This enforces horizontal, vertical, or 45° diagonal
line segments.

---

## Grid

**Command:** `Draft_Snap_Grid`

Snaps to the intersections of the Draft grid lines. The grid spacing is
set in **Edit → Preferences → Draft → Grid → Grid spacing**.

**Indicator:** Small yellow cross at the nearest grid point.

Grid snap is most useful for floor plan and schematic drawings where
coordinates align to a regular grid.

---

## Working Plane

**Command:** `Draft_Snap_WorkingPlane`

Projects any snap point onto the current working plane. When enabled,
snapped points that would normally be above or below the working plane
are pulled down to the plane.

**Indicator:** No separate visual — the point is projected silently.

This snap ensures all geometry is coplanar with the working plane even
when snapping to 3-D geometry above or below it.

---

## Dimensions

**Command:** `Draft_Snap_Dimensions`

Not a snap mode — rather, this toggle shows live **dimension feedback**
while drawing: the X, Y, and Z distances from the last point to the
current cursor position are displayed next to the cursor.

**Indicator:** Small dimension labels near the cursor showing Δx, Δy, Δz.

Enable this to verify distances while drawing without needing to check
the coordinate input boxes.

---

## Toggle Grid (Snap)

**Command:** `Draft_ToggleGrid`

Shows or hides the Draft grid (same as Draft → Utilities → Toggle Grid).
Also available in the snap toolbar for quick access while drawing.

---

## Snap configuration tips

### Recommended snap sets

**General 2-D drafting:**
Lock, Endpoint, Midpoint, Center, Intersection, Perpendicular, Ortho

**Architectural/floor plan:**
Lock, Endpoint, Midpoint, Center, Intersection, Grid, Perpendicular

**Following a path or curve:**
Lock, Endpoint, Midpoint, Center, Extension, Near

**Precision 3-D attachment:**
Lock, Endpoint, Center, Perpendicular, Working Plane

### Temporary snap override

While drawing, you can hold **Shift** to temporarily suppress all snapping
and place a free point. Release Shift to re-enable snapping.

### Snap sensitivity

The snap "bubble" radius (how close the cursor must be to trigger a snap)
is configured in **Edit → Preferences → Draft → Snap → Snap sensitivity**
(in pixels). Increase it for easier snapping in dense drawings; decrease
it to avoid unwanted snaps.

---

## Common mistakes and pitfalls

!!! warning "Snap Lock being off causes all snaps to fail"
    If snapping stops working entirely, check that the Snap Lock padlock
    icon is unlocked (pressed/active). The `S` shortcut is easy to hit
    accidentally during a drawing session.

!!! warning "Near snap overrides Endpoint"
    If Near is enabled and the cursor is near an endpoint, Near may snap
    to a point slightly off the endpoint. Disable Near when you need exact
    endpoint/midpoint placement.

!!! warning "Intersection snap needs both edges visible"
    Intersection snap only works when both edges are detected in the hover
    scan. If an edge is behind another object, FreeCAD may not detect it
    and the intersection snap will not fire. Rotate the view or zoom in.

!!! warning "Working Plane snap silently changes Z"
    If Working Plane snap is on and you snap to a 3-D point above the
    working plane, the placed point is projected down to Z=0 (the working
    plane). The point will not be at the height of the object you snapped
    to. Disable Working Plane snap if you want to pick true 3-D positions.

---

## See also

- [Select Working Plane](utilities.md#select-working-plane) — set the plane that Working Plane snap uses
- [Drafting Tools](drafting.md) — the tools that use snapping during input
- [Modification Tools](modification.md) — snapping used for Move, Rotate base points
