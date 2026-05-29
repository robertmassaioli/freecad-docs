# Beam Rotation

> **In one sentence:** Rotate the cross-section of beam elements about the
> beam axis — for example to orient an I-beam with its flanges horizontal.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Element Geometry → Beam Rotation

Rotates the cross-section of beam elements about the beam (longitudinal) axis.
By default the cross-section is oriented with one axis pointing toward global Z.
Use this tool when the cross-section must be rotated — for example an I-beam
with its web horizontal rather than vertical.

---

## Step-by-step

1. Select the beam edges whose cross-section orientation needs changing.
2. Choose **FEM → Model → Element Geometry → Beam Rotation**.
3. Enter the **Rotation** angle (degrees, right-hand rule about the beam axis).
4. Click **OK**.

---

## When you need it

- I-beams where the flanges must be in the horizontal plane.
- Angle (L) sections where the leg orientation affects bending stiffness.
- Any asymmetric cross-section (T, L, Z) where orientation matters.

---

## See also

- [Beam Cross Section](beam-cross-section.md) — define the cross-section shape
- [FEM Workbench](../index.md) — workbench overview
