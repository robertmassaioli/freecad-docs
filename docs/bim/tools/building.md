# Building

> **In one sentence:** Create a building container that holds one or more
> floor levels within a Site.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Building  
**Command:** `Arch_Building`  
**IFC class:** `IfcBuilding`

A Building container holds one or more Levels (storeys). It corresponds to
a single structure on the site. Most projects have one Building per Site;
multi-building campuses use multiple Building objects.

---

## Properties

| Property | Description |
|----------|-------------|
| `BuildingType` | IFC building type: OFFICE, RESIDENTIAL, INDUSTRIAL, etc. |

---

## Typical workflow

1. Create a [Site](site.md) first.
2. Go to **3D/BIM → Building**.
3. Drag the Building into the Site in the document tree.
4. Add [Level](level.md) objects inside the Building.

---

## See also

- [Site](site.md) — the geographic container holding this building
- [Level](level.md) — floor/storey container inside this building
- [BIM Workbench](../index.md) — workbench overview
