# Remove Components

> **In one sentence:** Delete all disconnected mesh components below a
> size threshold to clean up noise fragments from scans.

---

## Overview

**Workbench:** Mesh  
**Menu:** Mesh → Meshes → Remove Components…  
**Command:** `Mesh_RemoveComponents`  
**Shortcut:** none

Opens the Remove Components dialog showing all connected components (disconnected
mesh islands) sorted by triangle count. Components below a user-set threshold
are deleted automatically.

---

## Intuition

An imported scan often has many small noise fragments — stray triangles,
disconnected bits from occlusions, partial surfaces of nearby objects. Remove
Components eliminates all of them at once by setting a minimum size threshold.
Only components large enough (in triangle count) to be the main body are kept.

---

## When to use it

- After importing a scan mesh that has many small noise fragments.
- Cleaning up the output of [Split Components](split-components.md).
- Removing disconnected pieces left after a mesh Boolean or cut operation.

## When NOT to use it

- **Don't use it when some small components are important** — use
  [Remove Component by Hand](remove-component-by-hand.md) for selective deletion.

---

## Step-by-step

1. Select the mesh.
2. Choose **Mesh → Meshes → Remove Components…**.
3. The dialog shows all components sorted by triangle count.
4. Set the **threshold** (delete all components below this triangle count).
5. Click **Select** to preview, then **Delete** to remove.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Threshold | Remove all components with fewer triangles than this value |

---

## Common mistakes and pitfalls

!!! warning "This operation is irreversible without undo"
    Removing components modifies the mesh in place. Use **Ctrl+Z** immediately
    if you accidentally remove a component you need. After saving, there is
    no recovery.

!!! warning "The main body might be fragmented"
    If the mesh has gaps, the "main body" itself may be split into many
    components of similar size. Check the component list carefully before
    setting a threshold.

---

## See also

- [Remove Component by Hand](remove-component-by-hand.md) — selectively delete specific components
- [Split Components](split-components.md) — split into individual objects for manual management
- [Evaluate and Repair](evaluate-and-repair.md) — full mesh repair workflow
- [Mesh Workbench](../index.md) — workbench overview
