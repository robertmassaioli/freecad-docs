# Join Features

> **In one sentence:** Topology-aware Boolean operations for walled (hollow)
> solids that connect, embed, or cut openings while preserving the hollow
> interior.

---

## Overview

**Workbench:** Part · **Menu:** Part → Join → …  
**Toolbar:** Part Join · **Shortcut:** none

Standard Boolean Cut and Fuse treat solids as fully filled volumes. When applied
to hollow, thin-walled objects (pipes, housings, enclosures), they destroy the
hollow interior. The Join features are designed specifically for walled solids:
they understand the distinction between wall material and interior space, and
they maintain connectivity through that space.

| Tool | Description |
|------|-------------|
| [Connect Shapes](#connect-shapes) | Fuse two walled objects while connecting their interiors |
| [Embed Shapes](#embed-shapes) | Embed a smaller walled solid inside a larger one |
| [Cutout Shape](#cutout-shape) | Cut a hole matching an embedded object's profile |

---

## Intuition

Standard Boolean Fuse and Cut treat every solid as a completely filled volume.
That assumption breaks down for **hollow** objects — pipes, housings, and
thin-walled enclosures — because their defining feature is the *emptiness
inside* the wall, not just the wall itself.

If you Part Fuse two intersecting pipes, FreeCAD merges them into one solid but
fills the interior with material at the junction, destroying the flow path.
The Join tools understand the hollow character of walled solids and preserve it:

- **Connect** knows that the interior of both pipes is meant to be one
  continuous space; it opens the shared wall to connect them.
- **Embed** knows that one solid is being inserted *through* another's wall,
  not *into* its volume; it creates the opening without flooding the cavity.
- **Cutout** removes only the wall material at the penetration point, leaving
  the interior cavity intact.

Use these tools whenever you are building anything that is hollow: pipe runs,
electronic enclosures, sheet-metal boxes, or pressurised vessels.

---

## Connect Shapes

**Menu:** Part → Join → Connect  
**What it does:** Fuses two walled objects (pipes, tubes, shells) at a shared
boundary and opens the shared wall so their interiors communicate. The result
is a single hollow solid with a connected interior.

### Intuition

Welding a branch pipe onto a header pipe: the two pipes share a boundary face;
after joining, air (or fluid) can flow from one into the other. A plain Boolean
Fuse would fill the opening or produce degenerate internal faces. Connect Shapes
handles this correctly.

### When to use it

- Joining two pipes (T-junction, elbow-to-pipe, pipe-into-manifold).
- Connecting two sheet-metal enclosure bodies so their interiors are joined.
- Any two hollow solids where the cavity must remain connected.

### Step-by-step

1. **Select the first walled solid** (e.g., the main pipe).
2. **Ctrl+click the second walled solid** (e.g., the branch).
3. **Part → Join → Connect.**
4. The result is a single solid with the interiors connected.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | List of walled solids to connect (usually two) |
| Refine | Remove redundant edges from the result |

### Python API

```python
import FreeCAD as App
import BOPTools.JoinFeatures as JF

doc = App.newDocument()

# Create two intersecting tubes (simplified with cylinders)
tube1 = doc.addObject("Part::Cylinder", "Tube1")
tube1.Radius = 10; tube1.Height = 60

tube2 = doc.addObject("Part::Cylinder", "Tube2")
tube2.Radius = 8; tube2.Height = 40
tube2.Placement = App.Placement(
    App.Vector(0, 0, 30),
    App.Rotation(App.Vector(1, 0, 0), 90)
)
doc.recompute()

# Use JoinFeatures for actual walled solids
# (the above are solid cylinders for illustration)
j = JF.makeConnect(name="TJunction")
j.Objects = [tube1, tube2]
j.Proxy.execute(j)
doc.recompute()
```

---

## Embed Shapes

**Menu:** Part → Join → Embed  
**What it does:** Embeds a smaller walled solid inside a larger one by
cutting the outer solid's wall where the inner solid penetrates, and fusing the
two walls at the boundary. The larger solid's interior remains continuous around
the embedded object.

### Intuition

Inserting a pipe through a housing wall: the housing wall gets a circular
opening exactly matching the pipe's outer diameter; the pipe wall fuses with
the housing wall at the penetration. The housing interior is preserved.

### When to use it

- Inserting a boss or pipe through a housing wall.
- Embedding a connector flange into a panel.

### Python API

```python
import BOPTools.JoinFeatures as JF
import FreeCAD as App

doc = App.newDocument()

# outer_shell and inner_tube must be actual walled solids, not solid primitives
# For illustration, creating via Part Feature objects:
# outer = doc.addObject("Part::Feature", "OuterHousing")
# inner = doc.addObject("Part::Feature", "InsertedBoss")

j = JF.makeEmbed(name="EmbeddedBoss")
# j.Objects = [outer, inner]  # outer first, then inner
# j.Proxy.execute(j)
# doc.recompute()
print("Use interactively: select outer solid, Ctrl+click inner solid, "
      "then Part → Join → Embed.")
```

---

## Cutout Shape

**Menu:** Part → Join → Cutout  
**What it does:** Cuts a hole in a walled solid matching the cross-sectional
profile of an embedded object (the "tool"). Unlike Boolean Cut, which would
remove all material the tool overlaps, Cutout only removes the wall material
at the intersection — preserving the hollow interior of the host.

### Intuition

Cutting a porthole in a ship hull: you want to remove the hull wall at that
location (so the porthole can fit) but not destroy the interior cavity. Boolean
Cut would destroy the hollow interior. Cutout removes only the wall material,
leaving the correct opening.

### When to use it

- Cutting openings in housings or enclosures for connectors or tubes.
- Removing just the wall of a hollow body where another component passes through.

### Python API

```python
import BOPTools.JoinFeatures as JF
import FreeCAD as App

doc = App.newDocument()

j = JF.makeCutout(name="HousingCutout")
# j.Objects = [housing, tool_shape]  # host first, cutter second
# j.Proxy.execute(j)
# doc.recompute()
print("Use interactively: select walled solid, Ctrl+click tool shape, "
      "then Part → Join → Cutout.")
```

---

## Common mistakes and pitfalls

!!! warning "Join features require walled (hollow) solids"
    These tools are designed for objects with a wall separating an interior
    cavity from the exterior. Applied to fully solid objects, they produce
    results identical (or near-identical) to standard Boolean operations.

!!! warning "Overlap must exist at the boundary"
    The two objects must overlap through the wall region. If they only touch
    at a face without penetrating the wall, Join operations may produce
    unexpected results.

---

## See also

- [Boolean Fuse](boolean-fuse.md) — simpler union for solid objects
- [Boolean Cut](boolean-cut.md) — standard subtraction for solid objects
- [Split Features](split-features.md) — advanced splitting of any shapes
- [Thickness](thickness.md) — hollow out a solid to create a walled object
