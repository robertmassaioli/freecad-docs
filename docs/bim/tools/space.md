# Space

> **In one sentence:** Create a room or zone object that carries area,
> volume, and occupancy data within a building storey.

---

## Overview

**Workbench:** BIM  
**Menu:** 3D/BIM → Space  
**Command:** `Arch_Space`  
**IFC class:** `IfcSpace`

A Space represents a room, zone, or enclosed area within a building storey.
Spaces carry area and volume quantities (computed from the bounding shape),
IFC properties (occupancy, finish, department), and zone membership (thermal
zones, fire compartments).

---

## Creating a Space

A Space can be created from:

- **A closed solid** — the solid becomes the space's 3-D boundary.
- **A closed wire or face** — extruded to the storey height.
- **Automatically** — from the enclosing walls using the space detection algorithm.

---

## Properties

| Property | Description |
|----------|-------------|
| `Base` | The shape that defines the space boundary |
| `FinishFloor` | Floor finish material description |
| `FinishWalls` | Wall finish description |
| `FinishCeiling` | Ceiling finish description |
| `Area` | Computed floor area (read-only, mm²) |
| `Volume` | Computed enclosed volume (read-only, mm³) |

---

## See also

- [Level](level.md) — the storey container holding this space
- [Wall](wall.md) — walls that typically enclose a space
- [Schedule](schedule.md) — generate area schedules from spaces
- [BIM Workbench](../index.md) — workbench overview
