# Compound Tools

> **In one sentence:** Group multiple shapes into a single compound, split a
> compound back into individual shapes, or extract specific sub-shapes from a
> compound by type or index.

---

## Overview

**Workbench:** Part · **Menu:** Part → Compound → …  
**Toolbar:** Part Compound · **Shortcut:** none

A **compound** is an OCCT container that holds multiple shapes (solids, shells,
faces, edges, or mixes thereof) without merging them. The shapes inside remain
geometrically separate — no Boolean operation occurs. Compounds are useful for
grouping, for passing multiple shapes as a single object to other tools, or for
managing the output of Split operations.

| Tool | Description |
|------|-------------|
| [Make Compound](#make-compound) | Group shapes into one compound |
| [Explode Compound](#explode-compound) | Split a compound into individual shapes |
| [Compound Filter](#compound-filter) | Extract specific sub-shapes by type or index |

---

## Intuition

A compound is a lightweight grouping mechanism — a bag that holds shapes
without gluing them together. Think of it as a folder in the model tree at the
geometry level: you can put a shaft, a bushing, and a bearing in one compound
and move them all at once, without merging them into a single solid.

The critical distinction from Boolean Fuse is that a compound preserves internal
boundaries. The shaft, bushing, and bearing each remain separate solids inside
the compound. You can later extract them (Explode Compound), filter them
(Compound Filter), or pass the whole group as one input to another operation.

Compounds also arise naturally as the output of Split operations (Boolean
Fragments, Slice to Compound): instead of creating many individual model-tree
objects, the result is bundled into one compound for easy management.

---

## Make Compound

**Menu:** Part → Compound → Make Compound  
**What it does:** Takes all selected shapes and groups them into a single
`Part::Compound` object. The shapes are not merged — they remain separate
solids inside the container. Moving the compound moves all contained shapes
together.

### When to use it

- Grouping several parts for export as a single entity.
- Passing multiple shapes as a single input to a tool that expects one shape.
- Organising related shapes for visual clarity.

### Difference from Boolean Fuse

Fuse merges shapes into one solid (internal boundaries disappear). Make Compound
keeps them separate inside a container (internal boundaries remain).

### Step-by-step

1. **Select all shapes** to group (Ctrl+click each).
2. **Part → Compound → Make Compound.**
3. A `Part::Compound` object appears. The original shapes become children.

### Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 10; box.Width = 10; box.Height = 10

cyl = doc.addObject("Part::Cylinder", "Cyl")
cyl.Radius = 5; cyl.Height = 20
cyl.Placement = App.Placement(App.Vector(20, 0, 0), App.Rotation())
doc.recompute()

# Create compound programmatically
compound_shape = Part.makeCompound([box.Shape, cyl.Shape])
compound_feat = doc.addObject("Part::Feature", "MyCompound")
compound_feat.Shape = compound_shape
doc.recompute()

print(f"Solids in compound: {len(compound_feat.Shape.Solids)}")  # 2
```

---

## Explode Compound

**Menu:** Part → Compound → Explode Compound  
**What it does:** Splits a compound into its individual component shapes, each
becoming a separate `Part::Feature` object in the model tree.

### When to use it

- After [Boolean Fragments](split-features.md#boolean-fragments) or
  [Slice to Compound](split-features.md#slice-to-compound) to work with each
  fragment independently.
- After importing a STEP file that arrives as a compound of multiple bodies.
- When you need to hide, move, or further process each part of a compound
  independently.

### Step-by-step

1. **Select the compound** in the model tree.
2. **Part → Compound → Explode Compound.**
3. Individual Part Feature objects appear for each shape inside the compound.
4. The compound object itself remains; the new objects are separate copies.

### Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

# Create a compound with three shapes
shapes = [Part.makeBox(10, 10, 10),
          Part.makeCylinder(5, 20),
          Part.makeSphere(8)]
# Offset them so they don't overlap
shapes[1].Placement = App.Placement(App.Vector(20, 0, 0), App.Rotation())
shapes[2].Placement = App.Placement(App.Vector(40, 0, 0), App.Rotation())

compound_shape = Part.makeCompound(shapes)
compound_feat = doc.addObject("Part::Feature", "Compound")
compound_feat.Shape = compound_shape
doc.recompute()

# Explode: extract each solid into its own feature
for i, solid in enumerate(compound_feat.Shape.Solids):
    feat = doc.addObject("Part::Feature", f"Fragment_{i}")
    feat.Shape = solid
doc.recompute()
```

---

## Compound Filter

**Menu:** Part → Compound → Compound Filter  
**What it does:** Extracts specific sub-shapes from a compound, filtered by
shape type (Solid, Shell, Face, Edge, Vertex) and/or by index. The result is
a new compound containing only the matching sub-shapes.

### When to use it

- After Boolean Fragments: you want only the solids that are inside a certain
  bounding region.
- You have a compound of mixed shape types (solids + faces) and want only the
  solids.
- Selecting specific fragments by index for further processing.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Base | The source compound to filter |
| FilterType | All / Specific items / Collision passes shape / Window (bounding box) |
| Select Type | Shape type to match (Solid, Shell, Face, Edge, Vertex) |
| Items | Comma-separated indices of specific items (e.g. "0, 2, 5") |
| OverlapSubshape | Shape used as filter for Collision mode |
| OverlapValue | Threshold for bounding-box window filter |

### Filter modes

#### All

Returns all sub-shapes of the selected type — effectively a type filter only.

#### Specific items

Returns only the sub-shapes at the listed indices. Useful when you know which
fragments you want from a Boolean Fragments result.

#### Collision passes shape / Window

Filters sub-shapes by whether their bounding box overlaps with a reference
shape or falls within a bounding window. Useful for spatial selection of
fragments.

### Python API

```python
import FreeCAD as App
import CompoundTools.CompoundFilter as CF

doc = App.newDocument()

# Assume "compound_feat" exists from previous operations
# compound_feat = doc.getObject("Compound")

# The filter is typically used interactively.
# Programmatic equivalent: filter by type
# solids = [s for s in compound_feat.Shape.Solids]

# Or use CompoundFilter object:
# filt = CF.makeCompoundFilter(name="Filter")
# filt.Base = compound_feat
# filt.FilterType = "specific"
# filt.Items = "0, 1"
# doc.recompute()
print("Use Compound Filter interactively via Part → Compound → Compound Filter.")
```

---

## Common mistakes and pitfalls

!!! warning "Make Compound does not merge shapes"
    Make Compound groups shapes without merging. If you need a single merged
    solid, use [Boolean Fuse](boolean-fuse.md) instead.

!!! warning "Explode Compound creates copies, not the original children"
    The exploded features are new objects derived from the compound. Modifying
    them does not change the compound or vice versa.

---

## See also

- [Boolean Fragments](split-features.md#boolean-fragments) — produces a compound of fragments
- [Boolean Fuse](boolean-fuse.md) — merges shapes (unlike Compound)
- [Split Features](split-features.md) — split operations that produce compounds
