# Section Plane

> **In one sentence:** Place a cutting plane that generates 2-D orthographic
> views (plans, sections, elevations) from the 3-D model.

---

## Overview

**Workbench:** BIM  
**Menu:** Annotation → Section Plane  
**Command:** `Arch_SectionPlane`

Creates an `Arch::SectionPlane` — a cutting plane that generates 2-D
orthographic projections from the 3-D model. The plane can be oriented as a
plan (horizontal cut), section (vertical cut), or elevation (side view).

---

## How it works

1. Place the section plane at the desired cut elevation or orientation.
2. Set the `Objects` property to include all elements the section should cut.
3. Use **Drawing View** (`BIM_TDView`) to extract a 2-D projection onto a
   [Drawing Page](drawing-page.md).

---

## Key properties

| Property | Description |
|----------|-------------|
| `Objects` | List of document objects included in the section |
| `OnlySolids` | If True, cut only solid objects (ignore meshes and wires) |
| `Clip` | If True, clip all geometry to the plane's extent |
| `CutMargin` | Extra depth behind the cut plane to include in the view |

---

## Common mistakes and pitfalls

!!! warning "Section Plane shows no geometry"
    The `Objects` property is empty, or the objects are outside the plane's
    extent. Set `Objects` to include all elements you want sectioned, and
    ensure the plane intersects them.

---

## See also

- [Drawing Page](drawing-page.md) — TechDraw page to place the view onto
- [Drawing View](drawing-view.md) — add the section view to a drawing page
- [BIM Workbench](../index.md) — workbench overview
