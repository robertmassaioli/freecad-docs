# Scale Mesh Feature

> **In one sentence:** Scale a mesh by independent factors along X, Y, and Z,
> creating a scaled copy as a new `Mesh::Feature`.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Scale Mesh Feature  
**Shortcut:** none

Applies a non-uniform scaling to the selected mesh by multiplying each
coordinate by the specified factors `[sx; sy; sz]`. The original is hidden
and a new `Mesh::Feature` named `scale_<original>` is created.

---

## Intuition

Scale Mesh Feature is a dimensionless multiplier: `[2;1;0.5]` doubles the
X dimension, leaves Y unchanged, and halves the Z dimension. To scale to
absolute dimensions in mm, use [Resize Mesh Feature](resize-mesh-feature.md)
instead.

---

## When to use it

- You need to adjust the proportions of an imported mesh (e.g. a scanned
  object that was captured at a different scale).
- You want to create a scaled variant without losing the original.

---

## When NOT to use it

- **To set absolute dimensions** — use [Resize Mesh Feature](resize-mesh-feature.md).
- **For Part solids** — use Part → Scale.

---

## Step-by-step

1. Select a `Mesh::Feature`.
2. Choose **OpenSCAD → Scale Mesh Feature**.
3. Enter the scale vector `[sx;sy;sz]` in the dialog (default `[1;1;1]`).
4. Click **OK**. `scale_<name>` is created.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Scale vector | `[sx;sy;sz]` — dimensionless scale factors per axis. Default `[1;1;1]` is a no-op. |

---

## See also

- [Resize Mesh Feature](resize-mesh-feature.md) — scale to absolute target dimensions
- [Mirror Mesh Feature](mirror-mesh-feature.md) — reflect a mesh
- [OpenSCAD Workbench](../index.md) — workbench overview
