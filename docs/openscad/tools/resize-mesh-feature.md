# Resize Mesh Feature

> **In one sentence:** Scale a mesh so that its bounding box matches specified
> absolute target dimensions in mm.

---

## Overview

**Workbench:** OpenSCAD  
**Menu:** OpenSCAD → Resize Mesh Feature  
**Shortcut:** none

Computes the scale factors needed to make the mesh's bounding box match the
target `[lx;ly;lz]` dimensions, then applies uniform scaling. The original
is hidden and a `Mesh::Feature` named `resize_<original>` is created.

---

## Intuition

Where [Scale Mesh Feature](scale-mesh-feature.md) uses dimensionless multipliers,
Resize Mesh Feature uses absolute dimensions. You say "make this mesh 50 mm ×
30 mm × 20 mm" instead of "multiply by 2 in X".

---

## When to use it

- An imported mesh has incorrect dimensions (wrong units, wrong scale) and you
  need to set it to known absolute dimensions.
- You know the real-world dimensions of a scanned object and need to apply them.

---

## When NOT to use it

- **For dimensionless scaling** — use [Scale Mesh Feature](scale-mesh-feature.md).
- **For Part solids** — use Part → Scale.

---

## Step-by-step

1. Select a `Mesh::Feature`.
2. Choose **OpenSCAD → Resize Mesh Feature**.
3. Enter the target dimensions `[lx;ly;lz]` in mm.
4. Click **OK**. `resize_<name>` is created.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Target size | `[lx;ly;lz]` — target bounding box dimensions in mm |

---

## See also

- [Scale Mesh Feature](scale-mesh-feature.md) — scale by dimensionless factor
- [OpenSCAD Workbench](../index.md) — workbench overview
