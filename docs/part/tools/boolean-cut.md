# Boolean Cut

> **In one sentence:** Subtract one shape (the Tool) from another shape (the
> Base) to produce the difference.

---

## Overview

**Workbench:** Part · **Menu:** Part → Boolean → Cut  
**Toolbar:** Part Boolean · **Shortcut:** none

Boolean Cut performs the set-difference operation: the volume of the Tool shape
is removed from the Base shape. The result is a new solid containing only the
material that was in the Base but not in the Tool.

---

## Intuition

Drilling a through-hole is a Boolean Cut: the base is your block of metal, the
tool is a cylinder the diameter of the desired hole. Everything the cylinder
occupies is removed from the block. The cylinder "tool" itself is not destroyed
— it still exists in the model tree (set to invisible by default after the cut).

---

## When to use it

- Machining simulation: remove material from a billet.
- Creating through-holes, pockets, and slots in solid parts.
- Subtracting a body (e.g., a mould cavity) from a mould block.

## When NOT to use it

- **Inside a Part Design Body** — use Subtractive primitives or Pocket.
- **Cutting multiple shapes at once** — use
  [Boolean Fragments](split-features.md#boolean-fragments) or apply successive
  cuts.
- **The Tool does not intersect the Base** — Cut produces an unchanged copy of
  the Base. This is usually a sign of a wrong Placement.

---

## Step-by-step walkthrough

1. **Select the Base shape** (the shape to cut from).
2. **Ctrl+click the Tool shape** (the cutter).
3. **Part → Boolean → Cut** (or toolbar).
4. A `Part::Cut` object appears containing the result.
5. The original Base and Tool are hidden automatically.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base | The shape to cut from |
| Tool | The shape to subtract |

> **Note:** After creation, change Base and Tool by selecting the Cut object
> and editing the **Data** panel. The Refine property trims redundant edges
> from the result.

---

## Common mistakes and pitfalls

!!! warning "Tool does not intersect Base — empty result"
    If the Tool solid is entirely outside the Base, the Cut result is identical
    to the Base. Check that the Tool's Placement overlaps the Base's volume.

!!! warning "Reversed selection order gives a different result"
    `Base=box, Tool=cylinder` cuts a cylinder from a box (a box with a hole).
    `Base=cylinder, Tool=box` cuts a box from a cylinder (what remains of the
    cylinder outside the box). Order matters.

!!! warning "Non-manifold result when Tool is coplanar with Base face"
    If the Tool face exactly coincides with a Base face, the result may be
    non-manifold. Offset the Tool by a small epsilon past the face.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

# Base: 30×20×20 mm block
base = doc.addObject("Part::Box", "Block")
base.Length = 30; base.Width = 20; base.Height = 20

# Tool: 8 mm radius cylinder (through-hole cutter)
tool = doc.addObject("Part::Cylinder", "Cutter")
tool.Radius = 8.0
tool.Height  = 25.0  # longer than block to ensure full penetration
# Centre the cylinder on the block top
tool.Placement = App.Placement(
    App.Vector(15, 10, -2),  # XY centred, Z starts 2 mm below
    App.Rotation()
)
doc.recompute()

# Cut
cut = doc.addObject("Part::Cut", "BlockWithHole")
cut.Base = base
cut.Tool = tool
doc.recompute()

print(f"Volume before: {base.Shape.Volume:.2f} mm³")
print(f"Volume after:  {cut.Shape.Volume:.2f} mm³")
```

---

## See also

- [Boolean Fuse](boolean-fuse.md) — union of two shapes
- [Boolean Common](boolean-common.md) — intersection of two shapes
- [Boolean Dialog](boolean.md) — choose the operation type interactively
- [Refine Shape](copy-tools.md#refine-shape) — clean up redundant edges after cutting
