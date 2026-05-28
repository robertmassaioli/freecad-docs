# Mirror Mesh Feature

> **In one sentence:** Mirror a selected mesh about a user-specified axis,
> creating a mirrored copy as a new `Mesh::Feature`.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Mirror Mesh Feature  
**Shortcut:** none

Presents a dialog for selecting the mirror axis as a vector `[x;y;z]`. Preset
options include the three principal axes and diagonal combinations. The
original mesh is hidden and a new `Mesh::Feature` named `mirror_<original>` is
created with the mirrored geometry.

---

## Intuition

Mirroring a mesh is like placing it in front of a flat mirror and creating a
copy of the reflection. The mirror plane is perpendicular to the specified axis
and passes through the mesh's bounding box centre.

---

## When to use it

- You have a half-model of a symmetric part (scanned or imported) and want
  to reflect it to create the full symmetric shape.
- You need a mirrored copy for interference checking.

---

## When NOT to use it

- **For Part solids**, use Part → Mirror or Part Design → Mirrored — those
  produce proper B-rep geometry. This tool is for meshes only.

---

## Step-by-step

1. Select a `Mesh::Feature` object.
2. Choose **OpenSCAD → Mirror Mesh Feature**.
3. A dialog appears. Enter the mirror axis vector (e.g. `[1;0;0]` for X axis)
   or pick a preset.
4. Click **OK**. The original is hidden and `mirror_<name>` is created.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Axis vector | Mirror axis as `[x;y;z]`. The mirror plane is perpendicular to this axis. Presets: `[1;0;0]`, `[0;1;0]`, `[0;0;1]`, diagonals. |

---

## Python API

```python
import FreeCADGui as Gui
Gui.doCommand("Gui.runCommand('OpenSCAD_MirrorMeshFeature', 0)")
```

---

## See also

- [Scale Mesh Feature](scale-mesh-feature.md) — scale rather than mirror
- [OpenSCAD Workbench](../index.md) — workbench overview
