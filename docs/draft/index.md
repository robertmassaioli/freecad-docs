# Draft Workbench

> **In one sentence:** The Draft workbench provides 2-D parametric drawing
> tools — lines, arcs, curves, text, and dimensions — that work on an
> adjustable working plane, and a set of modification tools for transforming,
> patterning, and converting 2-D and 3-D geometry.

---

## What is the Draft workbench for?

Draft is FreeCAD's 2-D drawing workbench. Unlike Part Design (which builds
3-D solids) or Sketcher (which creates constrained sketches for features),
Draft is freeform: you draw lines, arcs, and shapes directly on a working
plane in 3-D space, then use modification tools to move, rotate, copy, or
pattern them.

**Common uses:**

- Producing 2-D technical drawings (floor plans, elevation views, diagrams).
- Creating reference geometry and construction lines for Part Design.
- Generating arrays of objects (linear, polar, circular, path-following).
- Annotating 3-D models with text labels and dimensions.
- Converting between geometry types (wire → B-spline, sketch → Draft, etc.).

---

## Key concepts

### The working plane

All Draft geometry is placed on the **working plane** — a flat reference
plane in 3-D space. By default it is the XY plane at Z = 0, but you can
change it to any face of a solid, any named plane (XZ, YZ, custom), or a
saved **Working Plane Proxy**.

Change the working plane with **Draft → Utilities → Select Working Plane**,
or pick a face in the 3-D view first and then activate a drawing tool.

### Snapping

Draft has a rich snapping system with 15+ snap types. When drawing, the
cursor snaps to geometric features of existing objects: endpoints, midpoints,
centres, intersections, perpendiculars, extensions, and more.

The **Snap toolbar** shows all active snap types. Toggle individual snaps
by clicking their buttons. The **Draft Snap Lock** (`Draft_Snap_Lock`) is
the master on/off switch.

### Layers

Draft objects can belong to **layers** — named groups that control the
visual style (line colour, line width, draw style, transparency) of all
members. Layers replace the older Freeze/Layer system. Manage layers with
**Utilities → Layer manager**.

### Construction mode

**Toggle Construction Mode** creates objects in a special "construction"
colour (blue by default). Construction objects are used as references and
hidden from the final output. Toggle with `Draft → Utilities → Toggle
Construction Mode`.

---

## Workbench layout

| Menu | Content |
|------|---------|
| **Drafting** | 2-D geometry creation: lines, arcs, circles, curves, text primitives |
| **Annotation** | Text, dimensions, labels, annotation style editor |
| **Modification** | Move, rotate, scale, mirror, trim, arrays, upgrade/downgrade, shape 2-D view |
| **Utilities** | Layers, groups, style, working plane, snapping controls |

---

## Typical workflow

```
1. Set the working plane (select face or named plane)
2. Enable snapping (Snap Lock on, pick snap types)
3. Draw geometry (Line, Arc, Circle, etc.) in the Drafting menu
4. Add annotations (Text, Dimension, Label) in the Annotation menu
5. Transform or pattern objects (Move, Rotate, Array) in Modification
6. Assign to layers (Layer Manager, Add to Layer) in Utilities
7. Export to SVG, DXF, or TechDraw page for 2-D output
```

---

## See also

- [Drafting Tools](tools/drafting.md) — all 2-D creation tools
- [Annotation Tools](tools/annotation.md) — text, dimensions, labels
- [Modification Tools](tools/modification.md) — transform, edit, convert
- [Array Tools](tools/arrays.md) — ortho, polar, circular, path arrays
- [Utility Tools](tools/utilities.md) — layers, groups, style, working plane
- [Snapping](tools/snapping.md) — snap types and the snap toolbar
