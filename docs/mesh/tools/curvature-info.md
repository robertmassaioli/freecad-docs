# Curvature Info

> **In one sentence:** Click a point on the mesh to report the principal,
> mean, and Gaussian curvature values at the nearest vertex.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Analyze → Curvature Info  
**Command:** `Mesh_CurvatureInfo`  
**Shortcut:** none

Activates a pick mode — click anywhere on the mesh to report the curvature
at the nearest vertex. Requires a [Vertex Curvature](vertex-curvature.md)
object to already exist in the document.

---

## Intuition

Curvature measures how sharply a surface bends. Flat faces have zero
curvature; cylinders have curvature in one direction; spheres have equal
curvature in all directions. Curvature Info lets you read the exact numerical
curvature at any point, useful for validating that a scanned surface is
truly flat or cylindrical.

---

## When to use it

- Validating a scan surface — confirming that a nominally flat region has
  near-zero curvature.
- Reading the radius of curvature of a cylindrical scan segment (R = 1/κ).
- Investigating isolated high-curvature points that may indicate noise or
  sharp defects.

## When NOT to use it

- **This tool requires Vertex Curvature to be computed first** — run
  [Vertex Curvature](vertex-curvature.md) before using Curvature Info.

---

## Step-by-step

1. Run [Vertex Curvature](vertex-curvature.md) first to compute curvature data.
2. Choose **Mesh → Meshes → Analyze → Curvature Info**.
3. Click any point on the mesh.
4. The curvature values appear in the Task panel.
5. Press **Escape** to exit.

---

## Reported values

| Value | Description |
|-------|-------------|
| Min curvature | Principal curvature κ₁ (lower value) |
| Max curvature | Principal curvature κ₂ (higher value) |
| Mean curvature | (κ₁ + κ₂) / 2 |
| Gaussian curvature | κ₁ × κ₂ |

**Reference values:**
- Flat face: κ₁ = κ₂ = 0
- Cylinder wall (radius R): κ₁ = 1/R, κ₂ = 0
- Sphere (radius R): κ₁ = κ₂ = 1/R

---

## Common mistakes and pitfalls

!!! warning "Requires Vertex Curvature object"
    If no Vertex Curvature object exists in the document, Curvature Info
    has no data to report. Run Vertex Curvature first.

---

## See also

- [Vertex Curvature](vertex-curvature.md) — compute the curvature data needed by this tool
- [Evaluate Facet](evaluate-facet.md) — inspect individual triangle properties
- [Mesh Workbench](../index.md) — workbench overview
