# Mesh from Shape (Gmsh)

> **In one sentence:** Generate a mesh using the external Gmsh mesher —
> required for boundary layers, physical groups, and fluid flow analysis.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh → Mesh from Shape (Gmsh)

Creates a mesh using Gmsh. Gmsh must be installed separately and its
executable path set in **Edit → Preferences → FEM → Gmsh**.

---

## Why use Gmsh over Netgen

- **Boundary layers** for fluid flow (viscous sublayer resolution).
- **Mesh field control** — grade density precisely across the domain.
- **Physical groups** — named regions for Elmer boundary conditions.
- **Structured hex meshes** for certain geometries.

---

## Step-by-step

1. Select the body.
2. Choose **FEM → Mesh → Mesh from Shape (Gmsh)**.
3. Set **Max Size**, **Min Size**, and **Order** (1 = linear, 2 = quadratic).
4. Choose a **Mesh Algorithm**.
5. Click **OK**.

---

## Mesh algorithms

| Algorithm | Best for |
|-----------|---------|
| Automatic | General use |
| Delaunay | Smooth geometries |
| Frontal | Thin features |
| HXT | Large meshes (fast) |

---

## Common mistakes and pitfalls

!!! warning "Gmsh binary not found"
    Install Gmsh and set the path in **Edit → Preferences → FEM → Gmsh → Gmsh binary**.

!!! warning "Elmer requires Gmsh mesh groups"
    For Elmer analyses, add [FEM Mesh Group](mesh-group.md) objects to name
    boundary faces before meshing.

---

## See also

- [Mesh from Shape (Netgen)](mesh-netgen.md) — built-in mesher, no install needed
- [FEM Mesh Boundary Layer](mesh-boundary-layer.md) — add boundary layers (Gmsh only)
- [FEM Mesh Group](mesh-group.md) — named face groups for Elmer
- [FEM Workbench](../index.md) — workbench overview
