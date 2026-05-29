# BIM Workbench

> **In one sentence:** The BIM workbench is FreeCAD's Building Information
> Modelling environment — it provides tools for creating architectural and
> structural geometry organised in a Site → Building → Level → Space hierarchy,
> with full IFC import/export and native IFC editing.

---

## What is the BIM workbench for?

BIM (**Building Information Modelling**) extends CAD into architecture and
construction: geometry is not just shape, but also carries semantic meaning
(this face is a wall, that element is a door), quantities (area, volume,
cost), and interoperability data (IFC class, property sets).

FreeCAD's BIM workbench provides:

- **Parametric building elements** — walls, columns, beams, slabs, doors,
  windows, stairs, roofs, pipes, frames, panels, trusses, and equipment.
- **Project structure** — Site → Building → Level → Space hierarchy that
  maps directly to IFC concepts.
- **IFC support** — import/export of Industry Foundation Classes (IFC) files,
  plus **Native IFC** editing that keeps the document as a real IFC model at
  all times (no conversion step).
- **Annotation and documentation** — section planes, 2-D views, dimensions,
  labels, schedules, and TechDraw integration.
- **Material and classification** — assign materials and IFC property sets
  to elements.

The BIM workbench incorporates and supersedes the older **Arch workbench**.
Most `Arch_*` commands are still available within BIM.

---

## IFC and Native IFC

**IFC** (Industry Foundation Classes) is the open standard format for BIM
data exchange. FreeCAD supports:

| Mode | Description |
|------|-------------|
| **Classic IFC** | Import an IFC file as FreeCAD objects; edit them natively; export back to IFC. |
| **Native IFC** | The FreeCAD document *is* the IFC file — every object maps directly to an IFC entity. Editing happens directly on the IFC model without a conversion step. |

Native IFC is the recommended mode for new projects that will be exchanged
with other BIM software. Classic IFC is useful for viewing or lightly editing
imported models.

---

## Project hierarchy

BIM objects are organised in a containment hierarchy:

```
Site
 └── Building
      └── Level (BuildingStorey)
           └── Space
                └── Wall / Column / Beam / Slab / Door / Window / ...
```

Each level corresponds to an IFC class:
- `Site` → `IfcSite`
- `Building` → `IfcBuilding`
- `Level` → `IfcBuildingStorey`
- `Space` → `IfcSpace`
- `Wall` → `IfcWall` / `IfcWallStandardCase`
- `Column` → `IfcColumn`
- etc.

---

## Menu structure

| Menu | Contents |
|------|----------|
| **2D Drafting** | Sketch, Line, Wire, Circle, Arc, Ellipse, Polygon, Rectangle, BSpline, BezCurve, Point |
| **3D/BIM** | Site, Building, Level, Space, Wall, CurtainWall, Column, Beam, Slab, Door, Window, Pipe, PipeConnector, Stairs, Roof, Panel, Frame, Fence, Truss, Equipment, Rebar; Generic 3D tools (Profile, Box, Builder, Library, Component, Reference) |
| **Annotation** | Text, ShapeString, Dimension (Aligned/Horizontal/Vertical), Leader, Label, Hatch, Axis, AxisSystem, Grid, SectionPlane, TDPage, TDView |
| **Snapping** | All Draft snap modes + working plane shortcuts |
| **Modify** | Move, Copy, Rotate, Clone, SimpleCopy, Compound; Offset2D, Trim/Extend, Join, Split, Scale, Stretch; Upgrade, Downgrade, Add, Remove; Array, PathArray, CutPlane, Mirror, Extrude, Boolean (Cut/Fuse/Common) |
| **Manage** | Setup, Views, ProjectManager, Windows, IfcElements, IfcQuantities, IfcProperties, Classification, Layers, Material, Schedule, Preflight |
| **Utils** | TogglePanels, WPView, Arch utilities (SplitMesh, MeshToShape, Check…), IFC utilities (IfcExplorer, IfcSpreadsheet, Diff) |

---

## See also

- [Site](tools/site.md) / [Building](tools/building.md) / [Level](tools/level.md) — project hierarchy
- [Wall](tools/wall.md) / [Column](tools/column.md) / [Slab](tools/slab.md) — structural elements
- [Section Plane](tools/section-plane.md) / [Drawing Page](tools/drawing-page.md) — construction documents
- [IFC Elements](tools/ifc-elements.md) / [Preflight](tools/preflight.md) — IFC data management
- [All Tools](tools/index.md) — complete tool reference
