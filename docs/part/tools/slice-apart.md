# Slice Apart

> **In one sentence:** Cut a Base shape using a Tool shape and create separate
> independent objects for each resulting fragment.

---

## Overview

**Workbench:** Part  
**Menu:** Part → Split → Slice Apart  
**Shortcut:** none

Cuts a Base shape using a Tool shape and replaces the Base with two or more
separate `Part::Feature` objects — one per fragment. Unlike
[Slice to Compound](slice-to-compound.md), the fragments become independent
model-tree objects rather than a compound.

---

## Intuition

Like using a bandsaw: the cut piece falls into two separate pieces that you can
pick up and move independently. The original Base shape is gone; in its place
are the individual fragments.

Use Slice Apart when the fragments need to be processed independently after
splitting — moved, hidden, filletted, or exported separately.

---

## When to use it

- Splitting a casting at a parting line to analyse mould halves independently.
- Cutting a solid into slices that will be placed on different layers or
  exported as separate files.
- Any case where you need the fragments as independent model-tree objects.

## When NOT to use it

- **Don't use it if you want to keep fragments together** — use
  [Slice to Compound](slice-to-compound.md), which keeps all fragments in one
  compound object.
- **Don't use it if you need to keep the original Base shape** — Slice Apart
  replaces the Base. Use [Slice to Compound](slice-to-compound.md), which is
  non-destructive.

---

## Step-by-step

1. Select the **Base shape** (the shape to cut).
2. **Ctrl+click** the **Tool shape** (the cutting shape — can be a face, wire,
   or solid that defines the cut plane/surface).
3. Choose **Part → Split → Slice Apart**.
4. The Base is replaced by two or more separate `Part::Feature` objects in the
   model tree.

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Base | The shape to be cut (replaced by fragments in the tree) |
| Tool | The cutting shape (remains in the tree unchanged) |

---

## Common mistakes and pitfalls

!!! warning "Base shape is replaced (destructive)"
    Unlike Slice to Compound, Slice Apart removes the original Base from the
    tree and replaces it with fragments. Use **Ctrl+Z** to undo if this is
    unintended.

!!! warning "Tool shape must intersect the Base"
    If the Tool shape does not cross the Base, no slice is performed and the
    result is the unchanged Base shape.

!!! warning "Tool can be a face or wire, not just a solid"
    Any shape that creates a cutting surface works as the Tool — a datum plane,
    a sketch, a flat face extracted from another solid.

---

## See also

- [Slice to Compound](slice-to-compound.md) — non-destructive version that keeps fragments in a compound
- [Boolean Fragments](boolean-fragments.md) — split all inputs at all mutual intersections
- [Boolean XOR](boolean-xor.md) — keep only non-overlapping portions
- [Boolean Cut](boolean-cut.md) — simple two-shape subtraction
- [Part Workbench](../index.md) — workbench overview
