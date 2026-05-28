# Set Default Orientation

> **In one sentence:** Set the default tool orientation applied to all
> subsequently inserted waypoints.

---

## Overview

**Workbench:** Robot  
**Menu:** Robot → Set Default Orientation  
**Shortcut:** none

!!! warning "Deprecated workbench"
    The Robot workbench is deprecated as of FreeCAD 1.0.

Stores the current tool frame orientation as the default for new waypoints.
All waypoints inserted after this call use this orientation unless explicitly
overridden.

---

## When to use it

- Most waypoints in a trajectory share the same approach angle (e.g. always
  perpendicular to a surface). Set the default once before recording waypoints.

---

## Step-by-step

1. Use the Robot Control panel to set the desired tool orientation.
2. Choose **Robot → Set Default Orientation**.
3. All subsequent Insert in Trajectory calls will use this orientation.

---

## See also

- [Set Default Values](set-default-values.md) — set default velocity
- [Insert in Trajectory (Tool Position)](insert-trajectory-tool-position.md)
- [Robot Workbench](../index.md) — workbench overview
