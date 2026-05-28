# Symbols

> **In one sentence:** TechDraw's symbol tools place standardised engineering
> notation — welding symbols, surface finish callouts, and ISO hole/shaft
> tolerance designations — directly on drawing views.

---

## Overview

**Workbench:** TechDraw  
**Menu:** TechDraw → Symbols

| Tool | Description |
|------|-------------|
| [Insert Welding Symbol](#insert-welding-symbol) | ISO 2553 / AWS welding symbol with leader |
| [Insert Surface Finish Symbol](#insert-surface-finish-symbol) | ISO 1302 surface roughness symbol |
| [Insert Hole / Shaft Fit](#insert-hole-shaft-fit) | ISO hole-and-shaft tolerance annotation |

---

## Intuition

These three tools automate the placement of engineering symbols that would
otherwise require memorising the correct graphical construction rules,
special characters, and standard layouts. Instead of drawing a weld symbol
by hand from individual line segments, Insert Welding Symbol presents a
structured form — you fill in the process, size, and position, and FreeCAD
builds the ISO-compliant symbol.

The symbols are **drawing objects** — they exist on the page only, linked to
a view via a leader line. They do not modify the 3-D model.

---

## Insert Welding Symbol

**Menu:** TechDraw → Symbols → Insert Welding Symbol

Creates an ISO 2553 / AWS A2.4 welding symbol composed of:

- A reference line (horizontal)
- An arrow line with arrowhead pointing at the weld location
- Weld symbols above and/or below the reference line
- Optional tail text (welding process, specification)
- Optional all-around and field-weld flags

### Step-by-step

1. Select a view on the page (the view that shows the joint to be welded).
2. Choose **TechDraw → Symbols → Insert Welding Symbol**.
3. The Welding Symbol task panel opens.
4. Fill in the fields:

   | Field | Description |
   |-------|-------------|
   | Arrow Side Symbol | Weld type symbol on the arrow side (fillet, groove, plug, etc.) |
   | Other Side Symbol | Weld type symbol on the other side |
   | Arrow Side Size | Weld size on the arrow side |
   | Other Side Size | Weld size on the other side |
   | Tail Text | Welding process or note (e.g. "SMAW" or "AWS D1.1") |
   | All Around | Toggle the all-around circle flag |
   | Field Weld | Toggle the field-weld flag |

5. Click **OK**. The symbol appears on the view. Drag the arrowhead to the
   weld location.

### Weld symbol types

| Symbol | Weld type |
|--------|-----------|
| Fillet | △ Fillet weld |
| Groove | ∨ or ∧ V-groove, bevel, U-groove |
| Plug | □ Plug or slot weld |
| Spot | ○ Spot or projection weld |
| Seam | = Seam weld |
| Back / Backing | ⌒ Backing or back weld |

### Python API

```python
import FreeCAD as App

doc = App.ActiveDocument
page = doc.getObject("Page")
view = doc.getObject("FrontView")

weld = doc.addObject("TechDraw::DrawWeldSymbol", "Weld1")
weld.Leader = leader_line_object   # requires a DrawLeaderLine parent
# Further configuration via task panel or properties
page.addView(weld)
doc.recompute()
```

---

## Insert Surface Finish Symbol

**Menu:** TechDraw → Symbols → Insert Surface Finish Symbol

Places an ISO 1302 surface texture / roughness symbol on a view. The
symbol consists of:

- The check-mark symbol (√) with optional lines indicating machining
  required or prohibited
- Ra roughness value
- Optional lay direction symbol
- Optional machining allowance

### Step-by-step

1. Select a view.
2. Choose **TechDraw → Symbols → Insert Surface Finish Symbol**.
3. In the task panel:

   | Field | Description |
   |-------|-------------|
   | Ra | Roughness average value (e.g. 1.6, 3.2, 6.3 μm) |
   | Symbol Type | Basic / Machining required / No machining |
   | Lay Direction | = // ⊥ × M C R (lay direction symbol) |
   | Method | Manufacturing method note |
   | Machining Allowance | Stock to remove (mm) |
   | Sampling Length | Sampling length for Ra measurement |

4. Click **OK**. Position the symbol on the face edge or place it with a
   leader.

### Ra standard values (ISO 1302)

| Ra (μm) | Typical process |
|---------|-----------------|
| 50 | Sand casting, rough cutting |
| 12.5 | Rough turning / milling |
| 6.3 | Normal turning / milling |
| 3.2 | Fine turning / milling |
| 1.6 | Grinding |
| 0.8 | Fine grinding |
| 0.4 | Honing / lapping |

---

## Insert Hole / Shaft Fit

**Menu:** TechDraw → Symbols → Insert Hole / Shaft Fit

Places an ISO 286 hole-and-shaft tolerance annotation on a dimension or
view. The annotation shows the tolerance designation (e.g. **H7/g6**) and
optionally the calculated upper and lower deviations.

### ISO tolerance system basics

ISO 286 defines fits using:
- A **fundamental deviation letter** (position of the tolerance zone relative
  to zero): uppercase for holes (A–ZC), lowercase for shafts (a–zc).
- A **IT grade number** (1–18): the width of the tolerance zone. Lower numbers
  are tighter.

Common fits:

| Designation | Type | Example use |
|-------------|------|-------------|
| H7/g6 | Clearance fit | Sliding bearing |
| H7/k6 | Transition fit | Gear on shaft |
| H7/p6 | Interference fit | Press fit |

### Step-by-step

1. Select a dimension (Diameter Dimension on a hole, for example) or a view.
2. Choose **TechDraw → Symbols → Insert Hole / Shaft Fit**.
3. In the task panel:
   - Select the **Hole Tolerance** (e.g. H7).
   - Select the **Shaft Tolerance** (e.g. g6).
   - Choose display format: symbol only, deviations only, or both.
4. Click **OK**.

The annotation appears attached to the selected dimension or as a standalone
annotation.

---

## Common mistakes and pitfalls

!!! warning "Welding symbol requires a leader first"
    The Insert Welding Symbol tool works best when you first place a Leader
    Line (TechDraw → Annotations → Insert Leader Line) on the view to define
    the arrow and reference line. The weld symbol attaches to the leader.

!!! warning "Surface finish Ra values are not enforced"
    FreeCAD accepts any numeric value for Ra — there is no enforcement of
    ISO preferred values. Always choose from the ISO 1302 preferred Ra series
    unless your standard explicitly uses non-standard values.

!!! warning "Hole/shaft fit tolerances require the nominal diameter"
    The tolerance deviation values (computed in μm from the nominal diameter
    and grade) depend on the nominal size. If the nominal diameter on the
    drawing is wrong, the computed deviations will also be wrong. Verify the
    nominal dimension before applying the fit annotation.

---

## See also

- [Annotations and Hatching](annotations.md) — leader lines, text annotations
- [Dimensions](dimensions.md) — diameter and length dimensions that symbols attach to
- Extensions — Centerlines / Threading — thread representations for mating features
