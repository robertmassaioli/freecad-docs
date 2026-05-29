# VTK Data Along Line

> **In one sentence:** Samples one or more result fields along a straight line
> through the mesh and displays the values as an X-Y plot.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Data Along Line

Places a sampling line between two user-defined points, interpolates the
result field at evenly-spaced positions along the line, and plots the
values as a graph. Used to extract through-thickness stress profiles, axial
temperature gradients, and cross-section velocity profiles.

---

## Step-by-step

1. Add a Data Along Line filter to the VTK pipeline.
2. Set the **Start Point** coordinates.
3. Set the **End Point** coordinates.
4. Set the **Resolution** (number of sample points along the line).
5. Choose the field(s) to plot.
6. A graph panel appears showing field value versus position (0–1 normalised
   along the line, or absolute distance).

---

## Use cases

- **Wall stress profile:** Sample Von Mises stress from the inside surface to
  the outside surface of a pressure vessel wall.
- **Thermal gradient:** Plot temperature from a hot surface to a cold surface
  to check the gradient against an analytical estimate.
- **Velocity profile:** Sample flow velocity across a channel to confirm a
  parabolic laminar profile.
- **Prerequisite for Linearised Stresses:** A through-thickness Data Along
  Line is required as input to the
  [Linearised Stresses](vtk-linearised-stresses.md) tool.

---

## See also

- [VTK Pipeline](vtk-pipeline.md) — create the pipeline first
- [Linearised Stresses](vtk-linearised-stresses.md) — membrane / bending stress decomposition along the line
- [Data at Point](vtk-data-at-point.md) — read a single-point value instead
- [FEM Workbench](../index.md) — workbench overview
