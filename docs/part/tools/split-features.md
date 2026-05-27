# Split Features

> **In one sentence:** Advanced Boolean operations that split shapes at mutual
> intersections rather than simply merging or subtracting them — producing
> multiple fragments, slices, or exclusive regions.

---

## Overview

**Workbench:** Part · **Menu:** Part → Split → …  
**Toolbar:** Part Split · **Shortcut:** none

Where standard Boolean Cut/Fuse/Common work on exactly two shapes and produce
one result, the Split features can handle multiple input shapes and produce
multiple output shapes. They are implemented as Python commands in
`BOPTools/SplitFeatures.py`.

| Tool | Description |
|------|-------------|
| [Boolean Fragments](#boolean-fragments) | Split all inputs at every mutual intersection |
| [Slice Apart](#slice-apart) | Cut a shape into separate objects using a tool |
| [Slice to Compound](#slice-to-compound) | Cut a shape and keep all fragments as one compound |
| [Boolean XOR](#boolean-xor) | Keep only the non-overlapping portions of two shapes |

---

## Intuition

Standard Boolean Cut and Fuse always produce *one* result shape, discarding
the pieces that are "removed". The Split tools are different: they **preserve
every fragment** — nothing is discarded.

Think of cutting a pizza: Cut gives you one slice and throws away the rest;
Slice gives you *all* the slices, keeping the topology of every piece. This
makes the Split tools essential for:

- **FEA / CFD meshing** — adjacent parts need conforming meshes (shared nodes
  at boundaries); Boolean Fragments splits all parts at their mutual boundaries
  so the mesh can be generated conformally.
- **Spatial analysis** — understanding exactly which regions two designs overlap
  (XOR) or how a complex body decomposes when cut by multiple planes.
- **Manufacturing planning** — slicing a solid into layers or halves for
  toolpath generation or mould split-line analysis.

The four tools differ in how many inputs they accept and whether the output
fragments become separate model-tree objects or stay bundled in a compound.

---

## Boolean Fragments

**Menu:** Part → Split → Boolean Fragments  
**What it does:** Takes any number of shapes, computes all pairwise
intersections, and splits every shape along every intersection boundary. The
result is a compound of all the non-overlapping fragments. No material is added
or removed — the total volume is preserved; it is just partitioned.

### Intuition

Imagine three overlapping spheres. Boolean Fragments divides them into seven
distinct regions: the three unique parts of each sphere, the three pairwise
overlaps, and the triple overlap. Each region becomes a separate solid in the
output compound.

### When to use it

- Analysing how multiple components overlap in an assembly.
- Preparing an FEA mesh where internal boundaries must be conforming (shared
  face between adjacent parts).
- Generating Voronoi-style tessellations or split patterns for design work.
- Cutting a shape with multiple cutting planes simultaneously.

### Step-by-step

1. **Select all the shapes** to fragment (any number).
2. **Part → Split → Boolean Fragments.**
3. A compound appears containing all fragments.
4. Use [Explode Compound](compound.md#explode-compound) to separate them into
   individual objects if needed.

### Parameters

| Parameter | Description |
|-----------|-------------|
| Objects | List of all input shapes |
| Mode | Standard / Split / CompSolid (controls how touching fragments are grouped) |

### Python API

```python
import FreeCAD as App
import BOPTools.SplitFeatures as SF

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 30; box.Height = 30

cyl = doc.addObject("Part::Cylinder", "Cyl")
cyl.Radius = 10; cyl.Height = 40
cyl.Placement = App.Placement(App.Vector(15, 15, -5), App.Rotation())
doc.recompute()

frags = SF.makeBooleanFragments(name="Fragments")
frags.Objects = [box, cyl]
frags.Proxy.execute(frags)
doc.recompute()

print(f"Number of fragments: {len(frags.Shape.Solids)}")
```

---

## Slice Apart

**Menu:** Part → Split → Slice Apart  
**What it does:** Cuts a Base shape using a Tool shape and creates **separate**
objects for each fragment — as opposed to [Slice to Compound](#slice-to-compound)
which keeps all fragments in one compound. The original Base is replaced; the
Tool remains.

### Intuition

Like using a bandsaw: the cut piece is split into two separate pieces that you
can move independently.

### When to use it

- You want the fragments as independent objects (separate model tree entries)
  that you can move, hide, or further process independently.
- Splitting a casting at a parting line to analyse mould halves.

### Step-by-step

1. **Select the Base shape** (the shape to cut).
2. **Ctrl+click the Tool shape** (the cutting shape — can be a face, wire, solid).
3. **Part → Split → Slice Apart.**
4. The Base is replaced by two or more separate Part Feature objects.

---

## Slice to Compound

**Menu:** Part → Split → Slice to Compound  
**What it does:** Cuts a Base shape using a Tool shape and keeps all resulting
fragments in a single compound object. The operation is non-destructive — the
original Base and Tool remain as separate objects.

### Intuition

Like Slice Apart, but the pieces stay bundled together in one container. Use
this when you want to keep them linked for further operations or export.

### When to use it

- Slicing geometry for export: you want one file containing all slices.
- Slicing a shape along a datum plane for FEA boundary conditions.

### Step-by-step

1. **Select the Base shape.**
2. **Ctrl+click the Tool shape.**
3. **Part → Split → Slice to Compound.**
4. A compound containing all fragments appears. Use
   [Explode Compound](compound.md#explode-compound) to split them later.

### Python API

```python
import FreeCAD as App
import BOPTools.SplitFeatures as SF

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 40; box.Width = 20; box.Height = 20

# Tool: a flat plane used as the cutting surface
plane = doc.addObject("Part::Box", "SlicePlane")
plane.Length = 50; plane.Width = 50; plane.Height = 0.001  # thin slab
plane.Placement = App.Placement(App.Vector(-5, -15, 10), App.Rotation())
doc.recompute()

sliced = SF.makeSlice(name="Sliced")
sliced.Objects = [box, plane]
sliced.Proxy.execute(sliced)
doc.recompute()
```

---

## Boolean XOR

**Menu:** Part → Split → Boolean XOR  
**What it does:** Computes the symmetric difference of two shapes — keeps only
the material that is in one shape **but not both**. The overlapping (shared)
region is removed. This is the inverse of Boolean Common.

### Intuition

XOR = Fuse − Common. The overlapping region that both shapes share is cut away;
only the unique parts of each shape remain.

### When to use it

- Cutting out the engagement region between two mating parts.
- Visualising the non-overlapping material when checking part fits.
- Generating the "difference region" for tolerance analysis.

### Step-by-step

1. **Select the first shape.**
2. **Ctrl+click the second shape.**
3. **Part → Split → Boolean XOR.**
4. A compound appears with the two non-overlapping fragments.

### Python API

```python
import FreeCAD as App
import BOPTools.SplitFeatures as SF

doc = App.newDocument()

s1 = doc.addObject("Part::Box", "Shape1")
s1.Length = 20; s1.Width = 20; s1.Height = 20

s2 = doc.addObject("Part::Box", "Shape2")
s2.Length = 20; s2.Width = 20; s2.Height = 20
s2.Placement = App.Placement(App.Vector(10, 0, 0), App.Rotation())
doc.recompute()

xor = SF.makeXOR(name="XOR")
xor.Objects = [s1, s2]
xor.Proxy.execute(xor)
doc.recompute()
print(f"XOR result solids: {len(xor.Shape.Solids)}")  # 2
```

---

## Common mistakes and pitfalls

!!! warning "Boolean Fragments on non-intersecting shapes"
    If none of the input shapes overlap, Boolean Fragments returns a compound
    of the original unchanged shapes. Check that inputs actually intersect.

!!! warning "Slice Apart modifies the model tree"
    Unlike Slice to Compound, Slice Apart replaces the Base object with the
    resulting fragments. The Base no longer exists as its original form. Undo
    with `Ctrl+Z` if this is unintended.

!!! warning "XOR of non-overlapping shapes = Fuse"
    When two shapes do not overlap, XOR returns both shapes unchanged (same as
    Fuse). Ensure the shapes share a volume for XOR to be useful.

---

## See also

- [Boolean Cut](boolean-cut.md) — simpler two-shape subtraction
- [Compound](compound.md) — managing compound output from split operations
- [Explode Compound](compound.md#explode-compound) — split compound fragments into separate objects
- [Join Features](join-features.md) — operations for walled/hollow solids
