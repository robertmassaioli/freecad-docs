# Inspection Workbench

> **In one sentence:** The Inspection workbench compares an *actual* object
> (a scan or manufactured part, represented as a mesh, point cloud, or B-rep)
> against one or more *nominal* objects (the design intent) and colours every
> point on the actual geometry by its signed deviation from the nominal.

---

## What is the Inspection workbench for?

The Inspection workbench answers the question: **"How closely does my
manufactured part match the design?"**

You supply:

- **Actual geometry** — the scanned or measured object. Can be a mesh
  (e.g. imported from a 3-D scanner), a point cloud, or a Part solid.
- **Nominal geometry** — the design intent. Can also be a mesh, point cloud,
  or Part solid.

FreeCAD computes the shortest distance from every point on the actual object
to the nearest point on the nominal, stores those distances on an
`Inspection::Feature` object, and displays a false-colour heat map directly in
the 3-D view: green near zero deviation, red/blue for positive/negative
deviations.

**Common uses:**

- First-article inspection: checking that a machined part is within tolerance
  of the CAD model.
- Reverse-engineering quality check: comparing a reconstructed surface to the
  original scan.
- Comparing two design revisions to see what changed geometrically.
- Detecting areas of a mesh that deviate from a reference shape.

---

## Workflow overview

```
Import actual scan ──► Import nominal (CAD model)
         │                          │
         └──────────┬───────────────┘
                    ▼
         Visual Inspection dialog
         (select Actual + Nominal,
          set Search Radius)
                    │
                    ▼
         Inspection::Feature created
         (Distances list attached)
                    │
                    ▼
         Colour map displayed in 3-D view
                    │
                    ▼
         Inspect Element (pipette)
         — hover to read per-point deviation
```

---

## Actual vs Nominal geometry

| Role | Meaning | Supported types |
|------|---------|-----------------|
| **Actual** | The real, manufactured part | Mesh (MeshObject), Point cloud, Part solid |
| **Nominal** | The design intent | Mesh (MeshObject), Point cloud, Part solid |

Any combination of types is supported. The most common pairing is:
Actual = mesh from 3-D scanner, Nominal = Part solid from Part Design.

---

## The distance sign convention

Distances are **signed**:

- **Positive** — the actual point is on the **outside** of the nominal surface
  (material is proud of the design, e.g. a burr or over-material condition).
- **Negative** — the actual point is on the **inside** of the nominal surface
  (material is missing, e.g. an undercut or under-material condition).
- **Zero** — the actual coincides with the nominal surface.

Points farther than **Search Radius** from any nominal surface receive a
special sentinel value (typically `float::max`) and are drawn in a distinct
colour (usually grey) to flag them as out-of-range.

---

## The colour bar

The Inspection result is visualised as a false-colour overlay:

| Colour | Meaning |
|--------|---------|
| Green | Near-zero deviation |
| Red | Positive deviation (material proud) |
| Blue | Negative deviation (material missing) |
| Grey | Outside Search Radius — no valid measurement |

The colour bar is visible in the 3-D view while an Inspection Feature is
selected. Drag the colour bar ends to rescale the range.

---

## See also

- [Visual Inspection](tools/visual-inspection.md) — set up and run an inspection
- [Inspect Element](tools/inspect-element.md) — hover-read per-point distances
