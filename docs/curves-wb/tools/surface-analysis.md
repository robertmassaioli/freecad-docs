# Surface Analysis

> **In one sentence:** Displays a false-colour map of surface curvature
> (Gaussian, mean, min, or max) over a face or shell.

---

!!! note "Third-party addon — installation required"
    The Curves workbench is not included in FreeCAD. Install it via
    **Tools → Addon Manager → search "Curves" → Install** and restart FreeCAD.

---

## Overview

**Workbench:** Curves  
**Menu:** Surfaces → Surface Analysis  
**Command:** `Curves_SurfaceAnalysis`

The colour scale runs from blue (low / negative) through green to red (high /
positive). A uniform colour indicates even curvature distribution.

---

## Curvature modes

| Mode | Description |
|------|-------------|
| **Gaussian curvature** | Product of principal curvatures; zero = developable surface |
| **Mean curvature** | Average of principal curvatures |
| **Min curvature** | Smaller principal curvature |
| **Max curvature** | Larger principal curvature |

---

## Common mistakes and pitfalls

!!! warning "Uniform colour on a visibly curved surface"
    The colour scale auto-ranges to the data. If curvature variation is small,
    even a curved surface can appear nearly uniform. Inspect the legend values
    — the surface may be nearly developable (low Gaussian curvature) even if
    it is visually curved.

---

## See also

- [Zebra Tool](zebra-tool.md) — visual continuity check
- [Draft Analysis](draft-analysis.md) — mould draft angle colour map
- [Flatten Face](flatten-face.md) — unroll developable surfaces (zero Gaussian curvature)
- [Curves Workbench](../index.md) — workbench overview
