# Downgrade

> **In one sentence:** Demotes shapes to a lower topological level — solid
> → faces, face → wires, compound → individual components.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Downgrade  
**Command:** `Draft_Downgrade`

The inverse of [Upgrade](upgrade.md):

| Input | Output |
|-------|--------|
| Solid | Faces (shell exploded) |
| Face | Wires (edges) |
| Compound | Individual component shapes |
| Two overlapping faces | Boolean cut result |

Useful for extracting individual faces from a solid for 2-D work.

---

## See also

- [Upgrade](upgrade.md) — promote shapes to higher topology
- [Shape 2D View](shape-2d-view.md) — project a 3-D shape to 2-D without demoting
- [Draft Workbench](../index.md) — workbench overview
