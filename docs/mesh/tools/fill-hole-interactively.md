# Fill Hole Interactively

> **In one sentence:** Click a hole boundary edge to fill that specific
> hole, giving precise control over which holes are closed.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Close Hole  
**Command:** `Mesh_FillInteractiveHole`  
**Shortcut:** none

Activates a pick mode where you click on a hole boundary edge to fill
that specific hole. Gives more control than [Fill Up Holes](fill-up-holes.md)
when the mesh has a mix of intentional openings and defect holes.

---

## Intuition

Some meshes have intentional openings (the open base of a vase, the open end
of a pipe) that should not be filled, while also having unintentional holes
from scan defects. Fill Hole Interactively lets you choose exactly which holes
to fill, leaving the intentional openings untouched.

---

## When to use it

- The mesh has a mix of intentional open boundaries and defect holes.
- You want to fill only specific holes, not all open boundaries.
- You need to verify each fill visually before confirming.

## When NOT to use it

- **Don't use it when you want to fill all holes at once** — use
  [Fill Up Holes](fill-up-holes.md) instead.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Close Hole**.
3. The open boundary edges are highlighted (red lines).
4. Click on any edge that borders the hole you want to fill.
5. The hole is filled immediately.
6. Repeat for additional holes.
7. Press **Escape** or click **Close** to exit.

---

## Common mistakes and pitfalls

!!! warning "Fill is immediate — no undo prompt"
    Each click fills the hole immediately. If you fill a hole accidentally,
    use **Ctrl+Z** to undo before proceeding.

!!! warning "The fill uses flat fan triangulation"
    Like Fill Up Holes, the interactive fill produces a flat patch. Visually
    inspect the filled area and manually add triangles ([Add Facet](add-facet.md))
    if needed for curved surfaces.

---

## See also

- [Fill Up Holes](fill-up-holes.md) — fill all holes automatically at once
- [Add Facet](add-facet.md) — add individual triangles for fine-grained repair
- [Evaluate and Repair](evaluate-and-repair.md) — full repair workflow
- [Mesh Workbench](../index.md) — workbench overview
