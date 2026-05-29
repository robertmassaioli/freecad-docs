# Section by Plane

> **In one sentence:** Create a cross-section wire at the intersection of a
> plane with the mesh — producing a set of line segments, not a trimmed mesh.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Cutting → Section by Plane  
**Command:** `Mesh_SectionByPlane`  
**Shortcut:** none

Creates a cross-section wire (a set of line segments) at the intersection of
a user-defined plane with the mesh. The mesh is not modified. The result is a
`Part::Feature` wire at the cut plane.

---

## Intuition

Section by Plane is the read-only version of [Trim by Plane](trim-by-plane.md):
it computes the intersection profile without cutting away anything. Use it to
extract a 2-D cross-section for measurement, reference in Sketcher, or CNC
toolpath generation.

---

## When to use it

- Extracting a 2-D profile from a scan mesh at a specific height.
- Creating a contour wire for CNC machining or reverse-engineering.
- Measuring the cross-sectional shape of an imported mesh.
- Providing an external geometry reference in Sketcher.

## When NOT to use it

- **Don't use it if you want to trim the mesh** — use
  [Trim by Plane](trim-by-plane.md) instead.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Cutting → Section by Plane**.
3. Pick three points to define the cutting plane, or use the task panel to
   set a point and normal vector.
4. A `Part::Feature` wire appears at the intersection.

---

## Common mistakes and pitfalls

!!! warning "Result is a wire, not a face or solid"
    The output is a set of line segments — not a face or solid. To use it as
    a sketch profile, switch to Sketcher and import the wire as external
    geometry.

!!! warning "Section through a hole or gap produces discontinuous segments"
    If the mesh has holes at the cut plane, the resulting wire will be
    discontinuous (open loops). Fill holes first with
    [Evaluate and Repair](evaluate-and-repair.md).

---

## See also

- [Cross-Sections](cross-sections.md) — create multiple parallel sections at once
- [Trim by Plane](trim-by-plane.md) — trim the mesh at a plane
- [Mesh Workbench](../index.md) — workbench overview
