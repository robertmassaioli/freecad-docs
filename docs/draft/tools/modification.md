# Modification Tools

> **In one sentence:** The Modification menu provides tools to move, rotate,
> scale, mirror, offset, trim, stretch, clone, edit, join, split, upgrade,
> downgrade, and convert Draft geometry, as well as pattern tools for
> creating arrays and 2-D projections of 3-D shapes.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification

| Tool | Description |
|------|-------------|
| [Move](#move) | Translate objects by a vector |
| [Rotate](#rotate) | Rotate objects around an axis |
| [Scale](#scale) | Scale objects relative to a base point |
| [Mirror](#mirror) | Mirror objects across a line |
| [Offset](#offset) | Offset a wire or face inward or outward |
| [Trim / Extend](#trim-extend) | Trim or extend a wire to a boundary |
| [Stretch](#stretch) | Move selected vertices while keeping connectivity |
| [Clone](#clone) | Create a linked parametric clone |
| [Edit](#edit) | Edit vertices of a Draft object interactively |
| [Subelement Highlight](#subelement-highlight) | Highlight a sub-element for editing |
| [Join](#join) | Join two open wires at their endpoints |
| [Split](#split) | Split a wire at a vertex |
| [Upgrade](#upgrade) | Promote shapes to higher topology |
| [Downgrade](#downgrade) | Demote shapes to lower topology |
| [Wire to B-Spline](#wire-to-b-spline) | Convert a polyline to a B-spline |
| [Draft to Sketch](#draft-to-sketch) | Convert Draft ↔ Sketcher objects |
| [Slope](#slope) | Set the inclination of a line |
| [Flip Dimension](#flip-dimension) | Flip a dimension text to the other side |
| [Shape 2D View](#shape-2d-view) | Project a 3-D shape onto the working plane |

---

## Move

**Menu:** Draft → Modification → Move  
**Command:** `Draft_Move`

Moves (or copies) selected objects by a displacement vector defined by
two picked points or explicit XYZ values.

### Step-by-step

1. Select the objects to move.
2. Activate **Draft → Modification → Move**.
3. Pick the **base point** (origin of the displacement).
4. Pick the **target point** (destination).
5. Optionally press **P** to make a copy instead of moving.

### Options during operation

| Key | Action |
|-----|--------|
| P | Toggle copy mode (make a copy, do not move original) |
| A | Toggle relative mode (target is offset from base) |
| G | Toggle global mode (use global coordinates) |

### Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
Draft.move([obj], App.Vector(50, 0, 0), copy=False)
App.ActiveDocument.recompute()
```

---

## Rotate

**Menu:** Draft → Modification → Rotate  
**Command:** `Draft_Rotate`

Rotates or copies selected objects by an angle around a centre point.
The rotation axis is perpendicular to the working plane.

### Step-by-step

1. Select the objects.
2. Activate **Draft → Modification → Rotate**.
3. Pick the **centre of rotation**.
4. Pick a point to set the **reference direction** (angle = 0°).
5. Pick a point to set the **rotation angle** (or type a value).

### Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
Draft.rotate(
    [obj],
    angle=45,
    center=App.Vector(0, 0, 0),
    axis=App.Vector(0, 0, 1),
    copy=False
)
App.ActiveDocument.recompute()
```

---

## Scale

**Menu:** Draft → Modification → Scale  
**Command:** `Draft_Scale`

Scales selected objects relative to a base point. Scale factors can be
specified for each axis independently (non-uniform scaling), or uniformly
(same factor on all axes).

### Parameters

| Parameter | Description |
|-----------|-------------|
| Base point | Point that stays fixed during scaling |
| X / Y / Z | Individual scale factors per axis |
| Uniform | Apply one factor to all axes equally |
| Copy | Make scaled copy rather than scaling in place |

### Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
Draft.scale(
    [obj],
    scale=App.Vector(2, 2, 1),   # X×2, Y×2, Z unchanged
    center=App.Vector(0, 0, 0),
    copy=True
)
App.ActiveDocument.recompute()
```

---

## Mirror

**Menu:** Draft → Modification → Mirror  
**Command:** `Draft_Mirror`

Creates a mirrored copy of selected objects across a mirror line defined
by two picked points. The mirror line lies in the working plane.

### Step-by-step

1. Select the objects.
2. Activate **Draft → Modification → Mirror**.
3. Pick the **first point** of the mirror line.
4. Pick the **second point** of the mirror line.

The mirror is always a copy — the original is preserved.

### Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
mirrored = Draft.mirror(
    [obj],
    p1=App.Vector(0, 0, 0),
    p2=App.Vector(0, 100, 0)
)
App.ActiveDocument.recompute()
```

---

## Offset

**Menu:** Draft → Modification → Offset  
**Command:** `Draft_Offset`

Creates a wire parallel to the selected wire at a specified offset
distance. The offset can be inward or outward (direction is determined
by the side you click).

### Behaviour

- For open wires: creates a parallel wire shifted by the offset distance.
- For closed wires or faces: creates a larger or smaller version.
- The offset distance is uniform (not variable per segment).

For complex shapes with variable offset, use Part Workbench → Offset 2D.

---

## Trim / Extend

**Menu:** Draft → Modification → Trim / Extend  
**Command:** `Draft_Trimex`

Trims a wire/edge back to a boundary edge, or extends it to meet a
boundary edge. The cursor side of the boundary determines whether the
wire is trimmed (cut) or extended (lengthened).

### Step-by-step

1. Activate **Draft → Modification → Trim / Extend**.
2. Click on the **wire to trim or extend** (click the part to keep for
   trim, or click the end to extend for extend).
3. Click on the **boundary edge** to trim/extend to.

The same tool handles both trim and extend — FreeCAD detects which
operation is needed based on whether the boundary is before or beyond
the clicked point.

---

## Stretch

**Menu:** Draft → Modification → Stretch  
**Command:** `Draft_Stretch`

Moves selected vertices of Draft wires while maintaining connectivity
with unselected vertices. This is useful for stretching one end or one
section of a drawing while keeping the rest fixed.

### Step-by-step

1. Activate **Draft → Modification → Stretch**.
2. Draw a selection rectangle around the vertices to move.
3. Pick the **base point** for the stretch displacement.
4. Pick the **target point**.

Only vertices that fall inside the selection rectangle are moved.
Adjacent segments to unselected vertices are stretched to maintain
connectivity.

---

## Clone

**Menu:** Draft → Modification → Clone  
**Command:** `Draft_Clone`

Creates a **parametric linked clone** of the selected object(s). The clone
follows the original — any change to the original shape is reflected in
the clone. The clone can be independently scaled and positioned.

### Clone vs Copy

| | Clone | Copy (Move with P) |
|--|-------|--------------------|
| Links to original | Yes — updates with original | No — independent |
| Individual scale | Yes — X/Y/Z independently | N/A |
| Can change shape | No — shape is always the original | Yes |

### Python API

```python
import Draft

obj = App.ActiveDocument.getObject("Wire")
clone = Draft.make_clone([obj], delta=App.Vector(50, 0, 0))
App.ActiveDocument.recompute()
```

---

## Edit

**Menu:** Draft → Modification → Edit  
**Command:** `Draft_Edit`

Opens a live editing session for a Draft object, showing its control
points (vertices for wires, handle points for splines and Béziers) as
draggable markers in the 3-D view.

### Supported objects

Draft_Edit works with: Draft Wire, Draft BSpline, Draft BezCurve,
Draft Circle, Draft Arc, Draft Ellipse, Draft Rectangle, Draft Polygon,
Draft Text, Draft Dimension, Draft Label.

It also works on Part objects by showing their vertices, though editing
is limited.

### Step-by-step

1. Select the Draft object.
2. Activate **Draft → Modification → Edit**.
3. The control points appear as small markers.
4. Drag a marker to reposition that point.
5. Right-click on a segment or point for additional context options
   (Add point, Delete point, Open/Close wire, etc.).
6. Press **Escape** or click outside to exit edit mode.

---

## Subelement Highlight

**Menu:** Draft → Modification → Subelement Highlight  
**Command:** `Draft_SubelementHighlight`

Temporarily highlights individual sub-elements (edges, vertices) of
objects, making them easier to snap to or select for further operations.
Useful when working with dense assemblies where selecting the correct
edge is difficult.

---

## Join

**Menu:** Draft → Modification → Join  
**Command:** `Draft_Join`

Joins two or more open Draft wires into a single wire by connecting their
endpoints. The wires must share or nearly share endpoints for the join to
succeed.

Select the wires to join (hold Ctrl), then activate the tool. If the
wires do not share endpoints, the join fails silently.

### Python API

```python
import Draft

wire1 = App.ActiveDocument.getObject("Wire")
wire2 = App.ActiveDocument.getObject("Wire001")
joined = Draft.join_wires([wire1, wire2])
App.ActiveDocument.recompute()
```

---

## Split

**Menu:** Draft → Modification → Split  
**Command:** `Draft_Split`

Splits a Draft wire at a selected vertex into two separate wires. The
split point must be an existing vertex of the wire (not a midpoint).

Select the wire, then activate **Draft → Modification → Split**, and
click on the vertex where you want to split.

---

## Upgrade

**Menu:** Draft → Modification → Upgrade  
**Command:** `Draft_Upgrade`

Promotes shapes to a higher topological level:

| Input | Output |
|-------|--------|
| Edges | Wire (if connected) |
| Closed wire | Face |
| Faces | Shell |
| Shells | Solid (if closed) |
| Overlapping faces | Fused solid (boolean union) |

Use Upgrade to build a face from lines drawn in the Draft workbench,
or to close a shell into a solid.

### Python API

```python
import Draft

edges = [App.ActiveDocument.getObject("Line"),
         App.ActiveDocument.getObject("Line001")]
results, failed = Draft.upgrade(edges, delete=True)
App.ActiveDocument.recompute()
```

---

## Downgrade

**Menu:** Draft → Modification → Downgrade  
**Command:** `Draft_Downgrade`

Demotes shapes to a lower topological level (the inverse of Upgrade):

| Input | Output |
|-------|--------|
| Solid | Faces (shell exploded) |
| Face | Wires (edges) |
| Compound | Individual component shapes |
| Two overlapping faces | Boolean cut result |

Useful for extracting individual faces from a solid for further 2-D work.

---

## Wire to B-Spline

**Menu:** Draft → Modification → Wire to B-Spline  
**Command:** `Draft_WireToBSpline`

Converts a Draft Wire (polyline) into a Draft BSpline using the same
vertex positions. The resulting spline is smooth through those points.

Conversely, select a Draft BSpline to convert it back to a polyline with
the control points as vertices.

---

## Draft to Sketch

**Menu:** Draft → Modification → Draft to Sketch  
**Command:** `Draft_Draft2Sketch`

Converts Draft objects to Sketcher sketch objects, or Sketcher sketch
objects to Draft objects.

**Draft → Sketch:** Useful when you need to apply Sketcher constraints
to a geometry originally drawn freehand in Draft. The result is an editable
Sketcher sketch in the same position.

**Sketch → Draft:** Useful for extracting geometry from a parametric
sketch into the Draft environment (e.g. to use with arrays or TechDraw
views).

!!! note
    Not all Draft geometry converts losslessly to Sketcher — complex
    B-splines may be approximated, and some construction geometry is lost.

---

## Slope

**Menu:** Draft → Modification → Slope  
**Command:** `Draft_Slope`

Sets the slope (rise/run ratio) of individual segments of a Draft Wire
or Line by modifying the Z coordinate of each vertex along the wire's
length. Used for drawing graded paths, drainage slopes, or ramps where
the horizontal projection is drawn flat but must have a specific gradient.

---

## Flip Dimension

**Menu:** Draft → Modification → Flip Dimension  
**Command:** `Draft_FlipDimension`

Flips the text of a selected Draft Dimension to the other side of the
dimension line. Useful when the auto-placed text overlaps with other
geometry.

---

## Shape 2D View

**Menu:** Draft → Modification → Shape 2D View  
**Command:** `Draft_Shape2DView`

Projects a 3-D solid or shape onto the working plane as a 2-D Draft wire
compound. The projection is orthographic (parallel rays perpendicular to
the working plane).

### Use cases

- Creating a flat 2-D plan view of a 3-D part for further Draft
  annotation.
- Extracting a cross-section line at a cut plane.
- Producing a 2-D outline for export to SVG or DXF.

### Projection modes

| Mode | Description |
|------|-------------|
| Solid | Visible and hidden outlines of the solid |
| Individual faces | Project one face only |
| Cutlines | Lines at the cut plane intersection |
| Cut faces | Filled faces at the cut plane |

The result is a `Draft::Shape2DView` object linked to the source solid.
If the solid changes shape, the 2-D view updates when the document is
recomputed.

### Python API

```python
import Draft

solid = App.ActiveDocument.getObject("Body")
view2d = Draft.make_shape2dview(
    solid,
    projectionVector=App.Vector(0, 0, 1)
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Clone scaling applies in local coordinates"
    The Clone's X/Y/Z scale properties scale along the clone's local axes,
    not the global axes. If the clone is rotated, scaling X does not scale
    along the global X direction. Always check the clone's orientation before
    applying non-uniform scaling.

!!! warning "Upgrade/Downgrade may not produce what you expect"
    Upgrade on disconnected edges creates separate wires, not one joined wire.
    The edges must be connected (share endpoints) to be joined into a wire.
    Use Join first to connect edges before upgrading to a face.

!!! warning "Shape 2D View does not update in real time"
    The Shape 2D View is recomputed when you trigger a document recompute
    (Edit → Refresh). It does not update automatically when the source solid
    changes.

!!! warning "Draft to Sketch loses construction geometry"
    Construction lines (blue lines from construction mode) are not converted
    when using Draft to Sketch. Only normal geometry is transferred.

---

## See also

- [Array Tools](arrays.md) — structured copies in patterns
- [Drafting Tools](drafting.md) — creating the geometry to modify
- [Snapping](snapping.md) — precise placement for move and rotate base points
