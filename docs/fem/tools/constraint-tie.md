# Tie Constraint

> **In one sentence:** Bond two faces permanently together so they move as
> one — even if they do not overlap or are not initially touching.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Mechanical Boundary Conditions → Tie Constraint

Bonds two faces by tying their nodes — the two faces always move together,
with no separation or sliding. Unlike [Contact](constraint-contact.md), Tie
is always active: once tied, the faces cannot separate.

---

## When to use Tie vs Contact

| Situation | Use |
|-----------|-----|
| Two faces that are always fully bonded (glued, welded, brazed) | Tie |
| Two faces that may separate or slide | [Contact](constraint-contact.md) |
| Mesh transition between coarse and fine regions | Tie |

---

## Tied mesh connections

Tie is commonly used to join two mesh regions with different element sizes —
for example, a refined mesh around a notch tied to a coarser mesh for the
rest of the body. The constraint interpolates between the two mesh densities
automatically.

---

## See also

- [Contact Constraint](constraint-contact.md) — contact that allows separation/sliding
- [FEM Workbench](../index.md) — workbench overview
