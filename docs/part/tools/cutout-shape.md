# Cutout Shape

> **In one sentence:** Cut a hole in a walled solid matching the penetration
> profile of another object, removing only the wall material while preserving
> the hollow interior.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Join → Cutout  
**Shortcut:** none

Removes only the wall material from a walled (hollow) solid where a tool shape
penetrates it. Unlike Boolean Cut, which removes all material the tool overlaps,
Cutout removes only the wall thickness at the intersection — leaving the host's
interior cavity intact.

---

## Intuition

Cutting a porthole in a ship hull: you want to remove the hull wall at that
location (so a frame or viewport can fit flush) but not destroy the interior
space. A Boolean Cut would subtract all material the cutting tool overlaps,
potentially removing deck space or internal structure. Cutout removes only the
hull wall material at the penetration point.

Another example: cutting a cable gland hole in an electronics enclosure — you
want a clean opening through the enclosure wall, not a hole through the interior
of the product.

---

## When to use it

- Cutting a hole in a housing or enclosure wall for a connector, cable gland,
  or pipe fitting.
- Cutting an opening in a sheet-metal box wall for a display panel or button.
- Any operation where you need a wall opening but must preserve the hollow
  interior of the host.

## When NOT to use it

- **Don't use it when the cut must go through the solid body** — use
  [Boolean Cut](boolean-cut.md) for fully solid objects.
- **Don't use it when the wall and the interior should merge** — use
  [Embed Shapes](embed-shapes.md) to fuse walls together.

---

## Step-by-step

1. Select the walled solid (the host with the wall to cut through).
2. **Ctrl+click** the tool shape (the object whose profile defines the opening).
3. Choose **Part → Join → Cutout**.
4. A new solid appears with the wall opening cut, interior preserved.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | Host walled solid first, then the tool shape |
| Refine | Remove redundant edges from the result |

---

## Common mistakes and pitfalls

!!! warning "Host must be a walled (hollow) solid"
    Cutout is designed for objects with an interior cavity. On fully solid
    objects, it behaves like a standard Boolean Cut.

!!! warning "Selection order matters"
    Select the host solid (the one being cut) first, then Ctrl+click the
    tool shape. Reversing the order produces the wrong result.

!!! warning "Tool must intersect the wall, not just the interior"
    If the tool shape is entirely inside the host without touching the wall,
    no opening is created. The tool must cross the wall boundary.

---

## Python API

```python
import FreeCAD as App
import BOPTools.JoinFeatures as JF

doc = App.newDocument()

j = JF.makeCutout(name="WallCutout")
# j.Objects = [housing_solid, tool_shape]   # host first, cutter second
# j.Proxy.execute(j)
# doc.recompute()
print("Use interactively: select walled solid, Ctrl+click tool shape, "
      "then Part → Join → Cutout.")
```

---

## See also

- [Connect Shapes](connect-shapes.md) — connect hollow interiors of two walled solids
- [Embed Shapes](embed-shapes.md) — embed an object through a wall with wall fusion
- [Boolean Cut](boolean-cut.md) — standard subtraction for solid objects
- [Thickness](thickness.md) — hollow out a solid to create a walled object
- [Part Workbench](../index.md) — workbench overview
