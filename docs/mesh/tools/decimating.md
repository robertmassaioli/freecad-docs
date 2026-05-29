# Decimating

> **In one sentence:** Reduce the number of triangles in the mesh while
> preserving the overall shape within a specified tolerance.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Decimate…  
**Command:** `Mesh_Decimating`  
**Shortcut:** none

Reduces the triangle count of the mesh while trying to maintain the surface
shape within a user-specified tolerance. Useful for reducing file size and
improving performance on over-detailed meshes.

---

## Intuition

A mesh from a fine scan or a high-resolution tessellation may have far more
triangles than needed for the intended use. A 3-D printer with 0.2 mm layer
height does not benefit from mesh triangles smaller than 0.2 mm — they just
inflate the file size and slow down the slicer. Decimation removes redundant
triangles while keeping the surface shape within the allowed tolerance.

---

## When to use it

- An STL from a high-resolution scan is too large for the slicer.
- You need to reduce file size for web or game use.
- After [Smoothing](smoothing.md), the mesh has many small triangles that
  represent noise that was smoothed away.

## When NOT to use it

- **Don't decimate below the print resolution** — if triangles become larger
  than 0.2 mm on a 0.2 mm layer-height printer, the mesh facets will be visible
  in the print.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Reduction (%) | Target reduction in triangle count |
| Tolerance | Maximum allowable deviation from the original surface (mm) |

Use **Tolerance** for engineering work — it maintains accuracy and stops
decimating when the error limit is reached. Use **Reduction %** when a
specific count target is needed.

---

## Common mistakes and pitfalls

!!! warning "Aggressive decimation introduces visible facets"
    Reducing below 10% of the original count can make flat panels visible
    on smooth surfaces. For 3-D printing, ensure triangles remain smaller
    than your layer height after decimation.

!!! warning "Decimation is not reversible without undo"
    Decimation modifies the mesh in place. Keep a backup copy before
    decimating.

---

## See also

- [Smoothing](smoothing.md) — reduce noise before decimating
- [From Part Shape](from-part-shape.md) — control triangle count at mesh creation time
- [Export Mesh](export-mesh.md) — export the decimated mesh
- [Mesh Workbench](../index.md) — workbench overview
