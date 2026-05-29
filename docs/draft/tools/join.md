# Join

> **In one sentence:** Joins two or more open Draft wires into a single wire
> by connecting their shared or nearly-shared endpoints.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Modification → Join  
**Command:** `Draft_Join`

Select the wires to join (hold **Ctrl**), then activate the tool. If the
wires do not share endpoints, the join fails silently.

---

## Python API

```python
import Draft

wire1 = App.ActiveDocument.getObject("Wire")
wire2 = App.ActiveDocument.getObject("Wire001")
joined = Draft.join_wires([wire1, wire2])
App.ActiveDocument.recompute()
```

---

## See also

- [Split](split.md) — split a wire at a vertex
- [Upgrade](upgrade.md) — promote joined edges to a face
- [Trim / Extend](trim-extend.md) — extend wires to meet
- [Draft Workbench](../index.md) — workbench overview
