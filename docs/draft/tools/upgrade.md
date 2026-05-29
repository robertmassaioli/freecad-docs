# Upgrade

> **In one sentence:** Promotes shapes to a higher topological level —
> edges → wire, closed wire → face, faces → shell, shells → solid.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Upgrade  
**Command:** `Draft_Upgrade`

| Input | Output |
|-------|--------|
| Connected edges | Wire |
| Closed wire | Face |
| Faces | Shell |
| Closed shell | Solid |
| Overlapping faces | Fused solid (boolean union) |

---

## Python API

```python
import Draft

edges = [App.ActiveDocument.getObject("Line"),
         App.ActiveDocument.getObject("Line001")]
results, failed = Draft.upgrade(edges, delete=True)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Disconnected edges are not joined"
    Upgrade on disconnected edges creates separate wires, not one joined wire.
    The edges must share endpoints to be joined. Use [Join](join.md) first
    to connect edges before upgrading to a face.

---

## See also

- [Downgrade](downgrade.md) — demote shapes to lower topology
- [Join](join.md) — connect edges before upgrading
- [Draft Workbench](../index.md) — workbench overview
