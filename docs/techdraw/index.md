# TechDraw Workbench

The TechDraw workbench creates traditional 2-D engineering drawings from
3-D models. It is the standard way to produce dimensioned, annotated,
title-block drawings in FreeCAD — the deliverable that goes to a machinist,
fabricator, or archive.

---

## What TechDraw is for

A 3-D model captures the geometry; a TechDraw drawing captures the
*communication* — which dimensions matter, what tolerances apply, which
surfaces need a finish, how the parts go together. TechDraw answers:

1. **How do I lay out views on a drawing sheet?** — front, top, right, section, detail.
2. **How do I add dimensions that stay linked to the model?** — change the model, the dimension updates.
3. **How do I annotate with notes, balloons, surface finish, and weld symbols?**
4. **How do I export to SVG, DXF, or PDF?**

---

## Key concepts

### Page and template

A TechDraw **Page** is the drawing sheet. Every page is based on a
**Template** — an SVG file that provides the border, title block, and any
fixed text. FreeCAD ships with ISO and ANSI templates in A0–A4 / A–E sizes.
You can create custom templates in Inkscape.

The template has **editable fields** (title, author, date, drawing number)
that you fill in via the Fill Template Fields tool without editing the SVG
directly.

### Views

A **View** is a 2-D projection of a 3-D object onto the drawing page.
Views are always *live* — they update automatically when the 3-D model
changes. The main view types are:

| View type | Description |
|-----------|-------------|
| Orthographic View | Standard front/top/right projection |
| Section View | Cross-section cut by a plane |
| Detail View | Magnified inset of a region |
| Projection Group | Sets of related orthographic views (first or third angle) |
| Broken View | Elongated part with middle section removed |

### Dimensions

Dimensions in TechDraw are placed on views and can be **linked to 3-D
geometry** so they read the true model dimension rather than the projected
length. A dimension linked to a Part Design pad length updates when the pad
length changes.

### The extension pack

TechDraw includes an **Extension Pack** — a large set of additional tools
for formatting dimensions, drawing centerlines, thread representations,
cosmetic geometry, and prefix/suffix manipulation. These are covered in the
Extensions reference page.

---

## Typical workflow

```
1. Create a Page          ← TechDraw → Page → Insert Default Page
2. Insert views           ← TechDraw → Views → Insert View (repeat)
3. Add a section view     ← TechDraw → Views → Insert Section View
4. Add dimensions         ← TechDraw → Dimensions menu (repeat)
5. Add annotations        ← TechDraw → Annotations menu
6. Fill title block       ← TechDraw → Page → Fill Template Fields
7. Export                 ← TechDraw → Page → Export Page as SVG / DXF
```

---

## Relationship to other workbenches

| Workbench | Relationship |
|-----------|-------------|
| **Part Design** | Bodies are the most common objects projected into TechDraw views. |
| **Part** | Part shapes and compounds can also be projected. |
| **Assembly** | Assembly views and exploded views project the full assembly. |
| **Spreadsheet** | A spreadsheet can be embedded as a table in a drawing. |
| **Draft** | Draft objects (2-D drawings) can be inserted as Draft Views. |

See the [Tools index](tools/index.md) for a complete reference.
