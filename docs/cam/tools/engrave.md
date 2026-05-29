# Engrave

> **In one sentence:** Follow selected edges or a ShapeString text object
> at a constant Z depth for engraving text, logos, and decorative patterns.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Engrave  
**Command:** `CAM_Engrave`  
**Shortcut:** none

Generates a toolpath that follows selected edges or a `Draft::ShapeString`
(text) object at a constant Z depth. The tool traces the path without varying
depth — for variable-depth engraving, use [V-Carve](v-carve.md).

---

## Intuition

Engrave is the simplest engraving operation: the tool traces each edge or
letter outline at a fixed depth. It is fast to compute and produces crisp
outlines at shallow depth, suitable for ID marks, part numbers, and simple
logos.

---

## When to use it

- Engraving text (part numbers, serial numbers) using a Draft ShapeString.
- Tracing a 2-D logo or decorative outline at constant depth.
- Scribing reference marks onto a part.

## When NOT to use it

- **For variable-depth engraving that fills wider regions** — use
  [V-Carve](v-carve.md) instead.

---

## See also

- [V-Carve](v-carve.md) — variable-depth carving with a V-bit
- [Deburr](deburr.md) — chamfer/deburring around face edges
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
