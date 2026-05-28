# Add OpenSCAD Element

> **In one sentence:** Type OpenSCAD code in a built-in editor, run it through
> the OpenSCAD binary, and import the resulting geometry into FreeCAD.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Add OpenSCAD Element  
**Requires:** OpenSCAD binary installed  
**Shortcut:** none

Opens a task panel with a code editor. You type (or paste) OpenSCAD code,
click **Add**, and FreeCAD calls the external OpenSCAD binary, converts the
result to STEP/CSG, and imports it into the document. Supports loading `.scad`
and `.csg` files.

---

## Intuition

Think of it as a mini OpenSCAD IDE embedded in FreeCAD. You write a `cube()`
or a complex parametric model, press Add, and the geometry appears in the
FreeCAD 3-D view without leaving FreeCAD. This is especially useful for
OpenSCAD-specific operations (Minkowski sum, hull, complex CSG) that don't have
direct equivalents in FreeCAD's native tools.

---

## When to use it

- You want to use OpenSCAD's `minkowski()`, `hull()`, or other CSG primitives
  directly.
- You are prototyping a shape in OpenSCAD and want to combine it with existing
  FreeCAD geometry.
- You have an existing `.scad` or `.csg` file to test-import.

---

## When NOT to use it

- **Without OpenSCAD installed** — the tool requires the OpenSCAD binary. If
  it is not configured, Add will fail with "executable not found".
- **For simple primitives** — use the Part workbench primitives (Box, Cylinder,
  etc.) directly; they are faster and don't require an external call.

---

## Step-by-step

1. Choose **OpenSCAD → Add OpenSCAD Element**.
2. The task panel opens with a code editor showing `cube();`.
3. Type or paste your OpenSCAD code.
4. Click **Add**. FreeCAD calls the OpenSCAD binary and imports the result.
5. To load from file: click **Open…** and select a `.scad` or `.csg` file.
6. To save the current code: click **Save…**.

!!! note
    Check **as mesh** if the OpenSCAD code produces non-manifold geometry.
    The result will be imported as a `Mesh::Feature` (STL) instead of CSG.

---

## Parameters

| Control | Description |
|---------|-------------|
| Code editor | OpenSCAD source code input |
| Add | Run OpenSCAD and import the result |
| Refresh | Clear the document and re-run Add |
| Open… | Load a `.scad` or `.csg` file into the editor |
| Save… | Save the current code to a file |
| Clear Code | Empty the code editor |
| as mesh | Import result as Mesh::Feature (STL) instead of CSG Part |

---

## Common mistakes and pitfalls

!!! warning "OpenSCAD executable not found"
    **Cause:** OpenSCAD is not installed or the path is not configured.  
    **Fix:** Install OpenSCAD from openscad.org. In FreeCAD: Edit →
    Preferences → OpenSCAD → set the path to the OpenSCAD binary.

!!! warning "Fails to load temp file (Snap/AppImage)"
    **Cause:** Sandboxed application packaging prevents FreeCAD and OpenSCAD
    from sharing `/tmp`.  
    **Fix:** In Edit → Preferences → OpenSCAD, change Transfer mechanism from
    "Temp directory" to an alternative shared path.

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_AddOpenSCADElement', 0)")
```

---

## See also

- [Mesh Boolean](mesh-boolean.md) — boolean operations using OpenSCAD's CGAL engine
- [Hull](hull.md) — convex hull via OpenSCAD
- [Minkowski Sum](minkowski-sum.md) — Minkowski sum via OpenSCAD
- [OpenSCAD Workbench](../index.md) — workbench overview
