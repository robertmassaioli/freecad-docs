# Site

> **In one sentence:** Create the top-level geographic container for a BIM
> project — the land parcel that holds one or more Buildings.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Site  
**Command:** `Arch_Site`  
**IFC class:** `IfcSite`

A Site is the top-level container for a BIM project. It represents the
physical land parcel and holds geographic information, one or more Buildings,
and site features such as terrain shape, perimeter, and landscaping.

---

## Intuition

The BIM hierarchy is **Site → Building → Level → Space → Elements**. Every
object must live somewhere in this tree for IFC export to work correctly.
The Site is the root of the tree — it anchors the project to a geographic
location.

---

## Step-by-step

1. Go to **3D/BIM → Site**.
2. FreeCAD creates an `ArchSite` object. Any selected objects become children.
3. In the Properties panel, set Latitude, Longitude, and Elevation.
4. Drag Building objects under the Site in the document tree.

---

## Properties

| Property | Description |
|----------|-------------|
| `Latitude` | Geographic latitude in decimal degrees |
| `Longitude` | Geographic longitude in decimal degrees |
| `Elevation` | Site elevation above sea level (mm) |
| `Declination` | Magnetic declination from north (degrees) |
| `Address` | Postal address string |
| `Terrain` | Link to a solid or surface representing the terrain shape |
| `ExportedIfc` | Path to the IFC file this site exports to (Native IFC mode) |

---

## Common mistakes and pitfalls

!!! warning "Site coordinates wrong in IFC export"
    Latitude and Longitude must be in **decimal degrees** (not
    degrees-minutes-seconds). A value of `51.5` is correct; `51°30'0"` is not.
    Convert DMS to decimal before entering.

---

## See also

- [Building](building.md) — building container inside the site
- [Level](level.md) — floor/storey container inside a building
- [Space](space.md) — room or zone inside a storey
- [BIM Workbench](../index.md) — workbench overview
