# Connect Shapes

> **In one sentence:** Fuse two hollow (walled) solids while opening their
> shared wall so their interior cavities form one continuous space.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Join → Connect  
**Shortcut:** none

Joins two walled objects (pipes, tubes, enclosures) at a shared boundary and
opens the shared wall so their interiors communicate. The result is a single
hollow solid with a connected interior.

---

## Intuition

Standard Boolean Fuse treats every solid as a completely filled volume. For
hollow objects — pipes, housings, thin-walled enclosures — that assumption
breaks down: a plain Fuse at a pipe junction fills the interior with material,
destroying the flow path.

Connect Shapes understands the hollow character of walled solids and preserves
it: when two pipes are connected, the shared wall is opened so fluid (or air,
or wire) can pass from one pipe into the other.

Think of it as welding a branch pipe onto a header: the walls fuse, and the
opening is cut through both walls at the junction.

---

## When to use it

- Joining two pipes at a T-junction, elbow, or wye.
- Connecting a branch pipe to a manifold.
- Joining two sheet-metal enclosure bodies so their interiors are continuous.
- Any two hollow solids where the internal cavity must remain connected.

## When NOT to use it

- **Don't use it for fully solid objects** — use [Boolean Fuse](boolean-fuse.md)
  instead. Connect Shapes adds no value when there is no interior to preserve.
- **Don't use it when the objects should not share an interior** — if you want
  two pipes side-by-side (touching but not joined), just place them without
  merging.

---

## Step-by-step

1. Select the first walled solid in the 3D view.
2. **Ctrl+click** the second walled solid.
3. Choose **Part → Join → Connect**.
4. A single solid result appears with connected interiors.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | List of walled solids to connect (usually two) |
| Refine | Remove redundant edges from the result |

---

## Common mistakes and pitfalls

!!! warning "Objects must be walled (hollow) solids"
    Connect Shapes is designed for objects with an interior cavity separated
    from the exterior by a wall. Applied to fully solid primitives, the result
    is indistinguishable from a standard Boolean Fuse.

!!! warning "Objects must overlap through the wall"
    The two solids must overlap at the wall region. If they only touch at a
    face without the wall intersection, Connect Shapes may produce unexpected
    results. Ensure there is genuine wall-to-wall overlap.

!!! warning "Very thin walls may cause tolerance issues"
    OCCT Boolean operations can fail or produce degenerate faces when walls
    are very thin relative to the model scale. Use consistent units and keep
    wall thickness above the modelling tolerance.

---

## Python API

```python
import FreeCAD as App
import BOPTools.JoinFeatures as JF

doc = App.newDocument()

# In practice, tube1 and tube2 should be actual hollow walled solids
# (e.g., shells or objects from the Mesh workbench converted to shells).
# The JoinFeatures API is:
j = JF.makeConnect(name="TJunction")
# j.Objects = [tube1, tube2]
# j.Proxy.execute(j)
# doc.recompute()
print("Use interactively: select both walled solids, then Part → Join → Connect.")
```

---

## See also

- [Embed Shapes](embed-shapes.md) — insert a walled solid through another's wall
- [Cutout Shape](cutout-shape.md) — cut a hole through a wall for a penetrating object
- [Boolean Fuse](boolean-fuse.md) — simpler union for fully solid objects
- [Thickness](thickness.md) — hollow out a solid to create a walled object
- [Part Workbench](../index.md) — workbench overview
