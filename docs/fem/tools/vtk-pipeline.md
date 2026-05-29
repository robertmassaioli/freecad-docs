# VTK Pipeline from Result

> **In one sentence:** Creates a VTK pipeline object from a result — the
> required first step for all advanced VTK post-processing in FEM.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Results → Pipeline from Result

Wraps a result object in a VTK dataset that subsequent filters can process.
This is the starting point for all VTK post-processing — all other VTK
tools (Clip, Contours, Calculator, etc.) are added as children of this
pipeline object.

---

## Requirements

The VTK post-processing tools require FreeCAD to be compiled with VTK support.
If the tool is greyed out, check **Help → About FreeCAD** for 'VTK' in the
build information.

---

## Step-by-step

1. Run the solver to create a result object in the model tree.
2. Select the result object.
3. Choose **FEM → Results → Pipeline from Result**.
4. A pipeline object appears in the tree.
5. Select the **Result field** to colour the mesh.
6. Add child filters (Clip, Contours, Calculator, etc.) to the pipeline.

---

## Common mistakes and pitfalls

!!! warning "VTK tools not available in all builds"
    The FreeCAD AppImage and most Linux package builds include VTK. Windows
    packages may not. Check Help → About FreeCAD for VTK in the build info.

---

## See also

- [Apply Changes](vtk-apply-changes.md) — force all pipeline filters to update
- [Warp by Vector](vtk-warp.md) — deform mesh by a vector result field
- [Clip Scalar](vtk-clip-scalar.md) — clip display at a scalar threshold
- [Contours](vtk-contours.md) — draw iso-contour surfaces
- [Show Results](results-show.md) — simpler alternative for basic colour maps
- [FEM Workbench](../index.md) — workbench overview
