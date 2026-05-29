# Material Editor

> **In one sentence:** Browse, create, and edit material property files in
> the FreeCAD material database.

---

## Overview

**Workbench:** FEM  
**Menu:** FEM → Model → Materials → Material Editor

Opens the FreeCAD material database editor — a browser for the built-in
material library and a form for viewing and editing all properties of any
material. Custom materials can be saved to `.FCMat` files and shared between
projects.

---

## What you can do

- Browse the library of built-in materials (metals, polymers, composites, fluids).
- View all properties of a selected material.
- Edit property values and save a custom material.
- Import/export `.FCMat` files.

---

## FCMat file format

```ini
[General]
Name = My Steel
Author = R. Smith
Description = Custom high-strength steel

[Mechanical]
YoungsModulus = 210000 MPa
PoissonRatio = 0.30
Density = 7900 kg/m^3
YieldStrength = 350 MPa
```

Custom materials are saved to the user material directory:
- **Linux:** `~/.local/share/FreeCAD/Material/`
- **Windows:** `%APPDATA%\FreeCAD\Material\`
- **macOS:** `~/Library/Application Support/FreeCAD/Material/`

---

## See also

- [Material for Solid](material-solid.md) — apply a material to an analysis body
- [FEM Workbench](../index.md) — workbench overview
