# Sketcher Tools

Complete index of all Sketcher workbench tools, grouped by function.

---

## Sketch Lifecycle

Tools for creating, editing, and managing sketch objects.

| Tool | Shortcut | Description |
|------|----------|-------------|
| [New Sketch / Edit Sketch / Leave Sketch](new-sketch.md) | — | Create a new sketch on a plane or face; enter/exit edit mode |
| [Map Sketch to Face](map-sketch.md) | — | Attach an existing sketch to a different face or plane |
| [Reorient Sketch](map-sketch.md#reorient-sketch) | — | Move a sketch to a different standard plane with optional offset |
| [Validate Sketch](validate-sketch.md) | — | Fix missing coincident constraints and degenerate geometry |
| [Mirror Sketch](mirror-sketch.md) | — | Create a mirrored copy of a sketch about one of its axes |
| [Merge Sketches](merge-sketches.md) | — | Combine two or more sketches into one |
| [View Sketch / View Section / Grid / Snap](view-sketch.md) | — | View helpers and display settings while editing a sketch |

---

## Geometry

Tools for drawing geometric elements inside a sketch.

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Point](create-point.md) | `G, Y` | Place a standalone point |
| [Line / Polyline](create-line.md) | `G, L` / `G, M` | Draw a single line segment or a connected chain of lines |
| [Arc (from center / from 3 points)](create-arc.md) | `G, A` / `G, 3, A` | Draw a circular arc |
| [Elliptical Arc](create-arc.md#elliptical-arc) | `G, E, A` | Draw an arc of an ellipse |
| [Hyperbolic / Parabolic Arc](create-arc.md#hyperbolic-and-parabolic-arcs) | `G, H` / `G, J` | Draw conic section arcs |
| [Circle (from center / from 3 points)](create-circle.md) | `G, C` / `G, 3, C` | Draw a circle |
| [Ellipse (from center / from 3 points)](create-circle.md#ellipse) | `G, E, E` / `G, 3, E` | Draw a full ellipse |
| [Rectangle / Centered Rectangle / Rounded Rectangle](create-rectangle.md) | `G, R` / `G, V` / `G, O` | Draw axis-aligned rectangles |
| [Regular Polygon](create-regular-polygon.md) | `G, P, 3` … | Draw regular polygons (triangle through octagon, or custom N) |
| [Slot / Arc Slot](create-slot.md) | `G, S` / `G, A, S` | Draw straight or arc-bounded slots |
| [Fillet / Chamfer](create-fillet.md) | — | Round or chamfer a corner between two lines |
| [B-Spline](create-bspline.md) | `G, B, B` | Draw B-splines by control points or interpolation, open or periodic |

---

## Editing

Tools for modifying existing sketch geometry.

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Trim / Split / Extend](trim-split-extend.md) | `G, T` / `G, Z` / `G, Q` | Trim, split, or extend edges to other geometry |
| [External Geometry / Carbon Copy](external-geometry.md) | `G, X` / `G, W` | Project or intersect 3-D geometry into the sketch; copy another sketch |

---

## Geometric Constraints

Constraints that enforce shape relationships without specifying numeric values.

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Coincident](constraint-coincident.md) | `C` | Force two points (or a point and a line) to share a location |
| [Horizontal / Vertical / HorVer](constraint-horizontal-vertical.md) | `H` / `V` / `A` | Force a line or pair of points to be horizontal, vertical, or the nearest of the two |
| [Parallel](constraint-parallel.md) | `P` | Force two lines to be parallel |
| [Perpendicular](constraint-perpendicular.md) | `N` | Force two lines to meet at 90° |
| [Tangent](constraint-tangent.md) | `T` | Force a line and a curve (or two curves) to be tangent |
| [Equal](constraint-equal.md) | `E` | Force two edges to have equal length or radius |
| [Symmetric](constraint-symmetric.md) | `S` | Force two points to be symmetric about an axis |
| [Block](constraint-block.md) | `K, B` | Fix an element in place without specifying distances |
| [Point on Object](constraint-point-on-object.md) | `O` | Force a point to lie on an edge or its extension |
| [Lock Position](constraint-lock.md) | `K, L` | Fix a point with explicit X, Y coordinates |

---

## Dimensional Constraints

Constraints that enforce numeric measurements (distance, angle, radius, …).

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Dimension (unified)](dimension.md) | `D` | Context-sensitive smart constraint: auto-detects distance, radius, or angle |
| [Distance](constraint-distance.md) | `K, D` | Set the straight-line distance between two points or edges |
| [Horizontal Distance](constraint-distance.md#horizontal-distance) | `L` | Set the horizontal distance between two points |
| [Vertical Distance](constraint-distance.md#vertical-distance) | `I` | Set the vertical distance between two points |
| [Radius / Diameter / Auto](constraint-radius-diameter.md) | `K, R` / `K, O` / `R` | Set the radius or diameter of a circle or arc |
| [Angle](constraint-angle.md) | `K, A` | Set the angle between two lines or a line and an axis |
| [Lock Position](constraint-lock.md) | `K, L` | Fix a vertex with absolute X, Y coordinates |
| [Toggle Driving / Toggle Active](toggle-constraint.md) | `K, X` / `K, Z` | Switch a constraint between driving and reference; enable or disable a constraint |
| [Snell's Law](constraint-snells-law.md) | `K, W` | Constrain refraction angle for optical path sketches |

---

## Transform Tools

Tools for creating arrays, mirrors, and transformed copies of sketch geometry.

| Tool | Shortcut | Description |
|------|----------|-------------|
| [Offset](offset.md) | `Z, T` | Create an equidistant closed contour around selected edges |
| [Move / Array Transform](translate.md) | `W` | Move geometry or create a rectangular array of copies |
| [Rotate / Polar Transform](rotate.md) | — | Rotate geometry or create a polar array of copies |
| [Scale](scale.md) | — | Scale geometry by a factor around a point |
| [Mirror (Symmetry)](symmetry.md) | — | Mirror selected geometry about a line in the sketch |
| [Copy / Clone / Move](copy-clone-move.md) | — | Duplicate geometry with or without constraint links |

---

## Analysis Tools

Tools for inspecting constraint status and managing geometry in bulk.

| Tool | Description |
|------|-------------|
| [Sketch Analysis](sketch-analysis.md) | Select redundant / conflicting / malformed constraints; select under-constrained elements; delete all geometry or constraints; remove axes alignment; toggle internal geometry |
