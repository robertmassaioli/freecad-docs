# Embed Shapes

> **In one sentence:** Embed a smaller walled solid inside a larger one,
> cutting the outer wall where the inner object penetrates and fusing the
> two walls at the boundary.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Join → Embed  
**Shortcut:** none

Embeds a smaller walled solid (the "tool") inside a larger walled solid (the
"host") by cutting an opening in the host's wall exactly matching the tool's
cross-section, and fusing the two walls at the boundary. The host's interior
cavity is preserved.

---

## Intuition

Imagine inserting a pipe through a housing wall: the housing wall gets a
circular opening exactly matching the pipe's outer diameter; the pipe wall
fuses with the housing wall at the penetration point; the housing interior
remains intact around the outside of the pipe.

A standard Boolean Fuse or Cut would either fill the housing interior or
destroy the pipe geometry. Embed Shapes understands the hollow structure and
performs only the correct wall-to-wall fusion.

---

## When to use it

- Inserting a pipe, tube, or boss through a housing wall.
- Embedding a connector flange into a panel enclosure.
- Inserting a cylindrical standoff through a sheet-metal box wall.

## When NOT to use it

- **Don't use it for fully solid objects** — use [Boolean Fuse](boolean-fuse.md)
  or [Boolean Cut](boolean-cut.md) instead.
- **Don't use it when the interiors should connect** — if the pipe and housing
  should share an internal cavity, use [Connect Shapes](connect-shapes.md)
  instead.

---

## Step-by-step

1. Select the outer (host) walled solid.
2. **Ctrl+click** the inner (embedded) walled solid.
3. Choose **Part → Join → Embed**.
4. The result shows the inner solid embedded through the outer wall.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | Outer solid first, then the inner solid to embed |
| Refine | Remove redundant edges from the result |

---

## Common mistakes and pitfalls

!!! warning "Both objects must be hollow"
    Embed Shapes requires walled (hollow) objects. Solid primitives do not have
    an interior cavity — the tool will behave like a standard Boolean on
    them.

!!! warning "Selection order matters"
    Select the outer (host) solid first, then Ctrl+click the inner (embedded)
    solid. Reversing the order produces the wrong result.

!!! warning "Inner solid must penetrate the outer wall"
    The inner solid must cross the outer wall boundary. If it is fully inside
    or fully outside without crossing the wall, the operation is undefined.

---

## Python API

```python
import FreeCAD as App
import BOPTools.JoinFeatures as JF

doc = App.newDocument()

# outer and inner must be actual walled solids (hollow shells), not solid primitives.
j = JF.makeEmbed(name="EmbeddedBoss")
# j.Objects = [outer_housing, inner_pipe]   # outer first, then inner
# j.Proxy.execute(j)
# doc.recompute()
print("Use interactively: select outer solid, Ctrl+click inner solid, "
      "then Part → Join → Embed.")
```

---

## See also

- [Connect Shapes](connect-shapes.md) — connect hollow interiors of two walled solids
- [Cutout Shape](cutout-shape.md) — cut only the wall, without fusing
- [Boolean Cut](boolean-cut.md) — standard subtraction for solid objects
- [Thickness](thickness.md) — hollow out a solid to create a walled object
- [Part Workbench](../index.md) — workbench overview
