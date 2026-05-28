# OpenSCAD Workbench

> **In one sentence:** The OpenSCAD workbench imports and exports OpenSCAD
> `.scad` / `.csg` files and, when the OpenSCAD binary is installed, adds
> convex-hull, Minkowski sum, and mesh boolean tools powered by OpenSCAD's
> robust geometry engine — plus a set of always-available shape-repair and
> tree-management utilities.

---

## What is the OpenSCAD workbench for?

[OpenSCAD](https://openscad.org/) is a script-based solid modeller that
produces geometry from a declarative description language. The FreeCAD
OpenSCAD workbench serves two roles:

1. **Bridge to OpenSCAD** — import `.scad` or `.csg` files as FreeCAD objects,
   and export FreeCAD geometry back to `.csg` for use in OpenSCAD.

2. **Utility toolkit** — a collection of shape-repair, mesh-manipulation, and
   document-tree tools that are useful regardless of whether OpenSCAD is
   installed.

The Mesh workbench's boolean operations internally use OpenSCAD's CGAL-based
boolean engine when available, because it is more robust for meshes than
OCCT's B-rep boolean operations.

---

## OpenSCAD binary requirement

Many tools in this workbench require the **OpenSCAD executable** to be
installed and its path configured in **Edit → Preferences → OpenSCAD**.

| Tool group | Requires binary |
|------------|----------------|
| Import .scad / .csg | Required |
| Add OpenSCAD Element | Required |
| Mesh Boolean | Required |
| Hull | Required |
| Minkowski Sum | Required |
| All other tools | **Not required** |

If the binary is not found at startup, FreeCAD prints a warning and the
binary-dependent tools are hidden from the menu and toolbar.

---

## Import and export

### Import

When the OpenSCAD binary is configured, FreeCAD registers the `.scad` and
`.csg` import filter. Use **File → Import** or drag-and-drop:

- `.scad` — full OpenSCAD source (FreeCAD calls OpenSCAD to convert to CSG first)
- `.csg` — OpenSCAD's intermediate CSG format (read directly)

The imported geometry appears as Part or Mesh features in the document tree,
depending on the OpenSCAD primitives used.

### Export

Use **File → Export** and choose **OpenSCAD CSG Format (*.csg)** to export
the selected shape. The exported file contains a CSG representation of the
shape's boundary.

---

## Preferences

**Edit → Preferences → OpenSCAD** exposes:

| Setting | Description |
|---------|-------------|
| **OpenSCAD executable** | Path to the `openscad` binary. Auto-detected on first launch if found in PATH. |
| **Transfer mechanism** | How FreeCAD passes data to OpenSCAD. Change if using Snap/AppImage packages that have sandbox restrictions. |
| **Mesh fallback quality** | Resolution used when exporting to STL for mesh boolean operations. |

---

## See also

- [OpenSCAD Tools](tools/index.md) — all tools
- OpenSCAD website: [openscad.org](https://openscad.org/)
