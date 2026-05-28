# Structure and IFC Hierarchy

BIM projects are organised in a containment hierarchy that maps directly to
the IFC (Industry Foundation Classes) building model:

```
Site  (IfcSite)
 └── Building  (IfcBuilding)
      └── Level  (IfcBuildingStorey)
           ├── Space  (IfcSpace)
           └── Building elements (Wall, Column, Slab, ...)
```

Each level is a **group-like container** — dragging elements into a container
sets the IFC containment relationship and controls visibility by level.

---

## Site {#site}

**Menu:** 3D/BIM → Site  
**Command:** `Arch_Site`  
**IFC class:** `IfcSite`

A Site is the top-level container for a project. It represents the physical
land parcel and can hold:
- Geographic information (latitude, longitude, elevation, address)
- One or more Buildings
- Site features (terrain shape, perimeter, landscaping)

### Properties

| Property | Description |
|----------|-------------|
| `Latitude` | Geographic latitude in decimal degrees |
| `Longitude` | Geographic longitude in decimal degrees |
| `Elevation` | Site elevation above sea level (mm) |
| `Declination` | Magnetic declination from north (degrees) |
| `Address` | Postal address string |
| `Terrain` | Link to a solid or surface representing the terrain shape |
| `ExportedIfc` | Path to the IFC file this site is exported to (Native IFC mode) |

### Step-by-step

1. Go to **3D/BIM → Site**.
2. FreeCAD creates an `ArchSite` object. Selected objects become children.
3. In the Properties panel, set Latitude, Longitude, and Elevation.
4. Drag the Building objects under the Site in the document tree.

---

## Building {#building}

**Menu:** 3D/BIM → Building  
**Command:** `Arch_Building`  
**IFC class:** `IfcBuilding`

A Building container holds one or more Levels. It corresponds to a single
structure on the site.

### Properties

| Property | Description |
|----------|-------------|
| `BuildingType` | IFC building type (e.g. OFFICE, RESIDENTIAL, INDUSTRIAL) |

### Typical use

Most projects have one Building per Site. Multi-building campuses can have
multiple Building objects in one Site.

---

## Level {#level}

**Menu:** 3D/BIM → Level  
**Command:** `Arch_Level`  
**IFC class:** `IfcBuildingStorey`

A Level (Building Storey) represents one floor of the building. It has an
elevation property and contains all elements at that floor.

### Properties

| Property | Description |
|----------|-------------|
| `Elevation` | Height of this storey above the building base (mm) |
| `Height` | Floor-to-floor height (used for display and space calculation) |

### Visibility control

Toggling a Level's visibility in the tree hides all elements on that floor —
useful for working on one storey at a time without visual clutter from other
floors.

---

## Space {#space}

**Menu:** 3D/BIM → Space  
**Command:** `Arch_Space`  
**IFC class:** `IfcSpace`

A Space represents a room, zone, or enclosed area within a building storey.
Spaces carry:
- Area and volume quantities (computed from the bounding shape)
- IFC properties (occupancy, finish, department)
- Zone membership (thermal zones, fire compartments)

### Creating a Space

A Space can be created from:
- A **closed solid** — the solid becomes the space's 3-D boundary.
- A **closed wire or face** — extruded to the storey height.
- **Automatically** — from the enclosing walls by running the space detection
  algorithm.

### Properties

| Property | Description |
|----------|-------------|
| `Base` | The shape that defines the space boundary |
| `FinishFloor` | Floor finish material description |
| `FinishWalls` | Wall finish description |
| `FinishCeiling` | Ceiling finish description |
| `Area` | Computed floor area (mm²) |
| `Volume` | Computed enclosed volume (mm³) |

---

## Common mistakes and pitfalls

!!! warning "Elements not appearing in the correct storey"
    **Cause:** Elements must be children of the Level in the document tree
    for the IFC containment to be correct.  
    **Fix:** Drag elements under the appropriate Level, or use
    **Manage → IFC Elements** to reassign containment.

!!! warning "Site coordinates are wrong in IFC export"
    **Cause:** Latitude and Longitude properties are not set, or are in the
    wrong unit (should be decimal degrees, not DMS).  
    **Fix:** Set `Latitude` and `Longitude` in the Site properties before
    exporting. Convert degrees-minutes-seconds to decimal degrees first.

---

## See also

- [Building Elements](elements.md) — walls, columns, slabs, etc.
- [IFC Management](ifc.md) — IFC classes and property sets
