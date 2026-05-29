# FEM Mesh Region

> **In one sentence:** Define a local mesh refinement region with a smaller
> element size around stress concentrations or areas of interest.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Mesh → FEM Mesh Region

Defines a local mesh refinement region. Nodes within the region use a smaller
element size than the global mesh.

---

## When to use it

- Stress concentrations: holes, fillets, notches, sharp re-entrant corners.
- Contact zones.
- Areas where temperature or potential gradients are steep.

---

## Step-by-step

1. Add a **Mesh from Shape (Gmsh)** or **Mesh from Shape (Netgen)** first.
2. Choose **FEM → Mesh → FEM Mesh Region**.
3. Select the face(s), edge(s), or vertex to refine around.
4. Enter the local **Max Element Size** (smaller than the global size).
5. Click **OK** and regenerate the mesh.

---

## Common mistakes and pitfalls

!!! warning "Coarse mesh underestimates peak stresses"
    A uniform coarse mesh misses steep stress gradients at holes, fillets,
    and notches. Add a Mesh Region with at least 3–5× smaller element size
    at every stress concentration.

---

## See also

- [Mesh from Shape (Gmsh)](mesh-gmsh.md) — recommended base mesher for regions
- [Mesh from Shape (Netgen)](mesh-netgen.md) — also supports regions
- [FEM Workbench](../index.md) — workbench overview
