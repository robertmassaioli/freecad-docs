# Sections

> **In one sentence:** Loft a smooth NURBS surface through a series of
> cross-section edges in order.

---

## Overview

**Workbench:** Surface  
**Menu:** Surface → Sections  
**Shortcut:** none

Creates a `Surface::Sections` object by interpolating a NURBS surface through
an ordered list of cross-section edges or wires. Each profile is a section;
the tool constructs the smoothest surface that passes through all of them in
sequence.

---

## Intuition

Imagine a ship hull drawn as a set of frame cross-sections, each one a curved
line at a different station along the hull's length. Sections stretches a
smooth NURBS skin through all those frames in order, interpolating the curves
between them. The result is a surface that passes exactly through every
section.

This is similar to Part Loft, but it produces a surface (not a solid) and
works with existing geometry edges rather than sketches.

---

## When to use it

- You have a series of cross-section curves (edges from existing geometry)
  and need the surface skin that connects them.
- You are doing surface-based styling (automotive, consumer products) where
  section curves are the design intent and the surface derives from them.
- You need the first or last edge to blend tangentially into an adjacent
  surface (G1 continuity at the ends).

---

## When NOT to use it

- **For a solid loft**, use Part Loft or Part Design Additive Loft — Sections
  produces a surface only.
- **For a 2-, 3-, or 4-curve patch**, [Fill Boundary Curves](fill-boundary-curves.md)
  or [Filling](filling.md) are more direct.

---

## Step-by-step

1. Select the cross-section edges in order from first to last (Ctrl+click).
   The order matters — the surface interpolates between them in the selection
   sequence.
2. Choose **Surface → Sections**.
3. In the task panel, verify the section list. Use the up/down arrows to
   reorder if needed.
4. For each section edge, use the reverse button (⇄) to flip its direction
   if the surface appears to twist.
5. Optionally set **Tangent edges** at the first and last sections for G1
   continuity with adjacent surfaces.
6. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| NSections | Reference list | — | Ordered list of cross-section edges/wires (first to last) |
| Tangent Edges | Reference list | — | Edges on the adjacent surface at the first or last section, used to enforce G1 continuity at the ends |

### Compared to Part Loft

| Feature | Surface Sections | Part Loft |
|---------|-----------------|-----------|
| Input | Edges and wires | Sketches and wires |
| Output | NURBS surface | Solid shell |
| End tangency | Yes (via tangent edges) | Yes (via profile tangency) |
| Closed result | No | Optional |
| Editable after creation | Yes (parametric) | Yes (parametric) |

---

## Common mistakes and pitfalls

!!! warning "Surface twists between profiles"
    **Cause:** Adjacent section curves run in opposite directions.  
    **Fix:** Use the reverse button (⇄) in the task panel to flip profiles
    until all run in the same direction.

!!! warning "Sections not in order"
    **Cause:** Sections were selected in an incorrect order and the surface
    folds back on itself.  
    **Fix:** Use the up/down arrows in the task panel to reorder the sections
    from one end of the loft to the other.

!!! warning "Tangent edge continuity has no effect"
    **Cause:** The adjacent surface at the tangent edge is planar or the
    tangent direction is perpendicular to the section.  
    **Fix:** G1 continuity only works when the tangent edge is on a smoothly
    curved surface and is geometrically compatible with the first or last section.

---

## Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
surf = doc.addObject("Surface::Sections", "Sections")

# NSections is a list of (object, edge_name) tuples
surf.NSections = [
    (doc.getObject("Sketch"), "Edge1"),    # first section
    (doc.getObject("Sketch001"), "Edge1"), # second section
    (doc.getObject("Sketch002"), "Edge1"), # third section
]
doc.recompute()
```

---

## See also

- [Fill Boundary Curves](fill-boundary-curves.md) — simpler for 2–4 boundary curves
- [Filling](filling.md) — arbitrary closed boundary with continuity control
- [Blend Curve](blend-curve.md) — smooth transition curve between two edges
- [Surface Workbench](../index.md) — overview of the Surface workbench
