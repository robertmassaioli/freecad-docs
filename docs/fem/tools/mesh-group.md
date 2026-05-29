# FEM Mesh Group

> **In one sentence:** Define a named group of mesh faces or elements —
> required for Elmer to associate boundary conditions with specific faces.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh → FEM Mesh Group

Defines a named group of mesh elements (faces, edges, volumes) that is
written into the Gmsh mesh file. Named groups are required by Elmer to
identify which boundaries receive which boundary conditions.

---

## Why it matters for Elmer

Elmer identifies boundary faces by group names in the mesh file. Without
named groups, Elmer cannot correctly assign boundary conditions. All Elmer
analyses should use Gmsh + FEM Mesh Group.

---

## Step-by-step

1. Add a Gmsh mesh first.
2. Choose **FEM → Mesh → FEM Mesh Group**.
3. Select the face(s) to include.
4. Enter a **Name** for the group (e.g. `inlet`, `wall`, `outlet`).
5. Click **OK** and regenerate the mesh.

---

## Common mistakes and pitfalls

!!! warning "Elmer requires named Gmsh mesh groups"
    If a Netgen mesh was used (which has no physical groups), Elmer cannot
    correctly apply boundary conditions. Regenerate with Gmsh and add Mesh Group objects.

---

## See also

- [Mesh from Shape (Gmsh)](mesh-gmsh.md) — required base mesher
- [Solver Elmer](solver-elmer.md) — the solver that uses group names
- [FEM Workbench](../index.md) — workbench overview
