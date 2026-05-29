# Deburr

> **In one sentence:** Generate a chamfering pass around the perimeter of a
> selected face to remove burrs left by other operations.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Deburr  
**Command:** `CAM_Deburr`  
**Shortcut:** none

Generates a chamfering pass that follows the boundary of a selected face.
The chamfer angle is determined by the V-bit or chamfer tool geometry; the
depth is shallow enough to remove the burr without machining the part surface.

---

## Intuition

Every milling operation leaves a small burr where the tool exits the material.
Deburr traces the face perimeter with a chamfer tool at a slight Z offset to
clean these edges. It is almost always the last operation in a programme.

---

## When to use it

- Removing burrs from profiled or pocketed edges.
- Adding a light chamfer to part edges as a finishing touch.
- Cleaning up edges that are hidden from normal Profile/Pocket operations.

## When NOT to use it

- **For variable-depth ornamental engraving** — use [V-Carve](v-carve.md).
- **For constant-depth text or logo tracing** — use [Engrave](engrave.md).

---

## See also

- [Engrave](engrave.md) — constant-depth edge tracing
- [V-Carve](v-carve.md) — variable-depth V-bit carving
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
