# Surface

> **In one sentence:** Generate a 3-D surface machining toolpath across
> arbitrary curved surfaces using parallel passes at a specified step-over.

---

## Overview

**Workbench:** CAM  
**Menu:** CAM → Surface  
**Command:** `CAM_Surface`  
**Shortcut:** none

!!! note "Requires OpenCamLib"
    Surface requires the OpenCamLib (OCL) library to be installed. Without
    it, the operation will not function.

Generates a 3-D surface machining toolpath by tracing the model surface at a
specified step-over distance. Suitable for 3-axis free-form surface milling
with ball-nose or bull-nose end mills.

---

## Intuition

Where [Pocket Shape](pocket-shape.md) works at fixed Z levels, Surface follows
the actual 3-D shape of the model — the tool moves up and down as needed to
stay in contact with the surface. This produces much smoother surfaces on
curved, organic geometry.

---

## When to use it

- Finishing curved surfaces (moulds, organic shapes, artistic forms).
- 3-D surface machining with ball-nose end mills.
- Producing a fine surface finish on 3-D contours.

## When NOT to use it

- **Requires OpenCamLib** — if not installed, this operation is unavailable.
- **For flat or 2.5D geometry** — use [Profile](profile.md) or
  [Pocket Shape](pocket-shape.md) instead.

---

## Scan types

| Type | Description |
|------|-------------|
| Planar | Passes parallel to the XY plane (waterline-style at any angle) |
| Rotational | Revolving passes for turning or 4-axis machining |

---

## See also

- [Waterline](waterline.md) — horizontal constant-Z passes for steep surfaces
- [3D Pocket](3d-pocket.md) — clearing curved-floor pockets
- [Common Parameters](common-parameters.md) — depths, heights, tool controller
- [CAM Workbench](../index.md) — workbench overview
