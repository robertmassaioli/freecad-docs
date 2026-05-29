# Cross-Sections

> **In one sentence:** Create multiple parallel cross-section wires through
> the mesh at regular intervals along a chosen axis.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Cutting → Cross-Sections  
**Command:** `Mesh_CrossSections`  
**Shortcut:** none

Creates a set of evenly spaced parallel section wires through the mesh.
This is the batch version of [Section by Plane](section-by-plane.md) — it
generates multiple sections in one operation.

---

## Intuition

Where Section by Plane gives you one cross-section, Cross-Sections gives you
many — like slicing a loaf of bread. Each slice is a wire showing the mesh
profile at that height. This is useful for terrain contour maps, layer-by-layer
print planning, or systematic inspection of an object's shape.

---

## When to use it

- Generating contour lines of a terrain mesh for mapping.
- Creating layer-by-layer profiles for large-format printing or CNC roughing.
- Inspecting the internal shape of a closed mesh at multiple heights.
- Building a stack of cross-sections for reverse engineering.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Cutting → Cross-Sections**.
3. Set the parameters (see below).
4. Click **OK**. A set of `Part::Feature` wires appears.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Plane | XY, XZ, or YZ — orientation of the cutting planes |
| Position (start) | First cutting plane position along the normal axis (mm) |
| Position (end) | Last cutting plane position (mm) |
| Count | Number of sections between start and end |
| Connections | Connect adjacent section contours with line segments |

---

## Common mistakes and pitfalls

!!! warning "Results are wires, not faces"
    Each cross-section is a set of line segments. To use them as sketch
    profiles, import the wires as external geometry in Sketcher.

!!! warning "Large count values generate many objects"
    Setting Count = 1000 creates 1000 Part::Feature objects in the tree.
    Keep counts reasonable for interactive use.

---

## See also

- [Section by Plane](section-by-plane.md) — single cross-section
- [Mesh Workbench](../index.md) — workbench overview
