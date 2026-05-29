# Dress-up: Axis Map

> **In one sentence:** Remap one motion axis to another — for example Z to Y
> for a horizontal spindle, or X to A for a 4-axis rotary setup.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Dress-up → Axis Map  
**Command:** `CAM_DressupAxisMap`  
**Shortcut:** none

Remaps motion axis assignments in the toolpath. This allows a toolpath
generated for a standard vertical mill (X/Y/Z) to be used on machines with
different axis conventions — such as a horizontal machining centre (where the
spindle axis is Y rather than Z), or a 4-axis machine with a rotary A axis.

---

## Intuition

FreeCAD generates toolpaths assuming the spindle moves along Z. If your
machine has a different configuration, the Axis Map dress-up swaps the
axis letter codes in the output G-code without changing the geometry.

---

## When to use it

- Horizontal machining centres where the spindle axis is Y or X.
- 4-axis rotary setups where one linear axis is replaced by a rotary axis.
- Machines with non-standard axis naming conventions.

---

## See also

- [Dressup: Z Correct](dressup-z-correct.md) — height map correction for workpiece warp
- [CAM Workbench](../index.md) — workbench overview
