# Workbench Documentation Proposal

**Status:** Active planning document  
**Purpose:** Track which workbenches should be documented next, in priority order,
with rationale. Update this file as workbenches are completed.

---

## Completed workbenches

| Workbench | Sprint completed | Pages |
|-----------|-----------------|-------|
| Part Design | Sprint 1 | ~40 pages |
| Sketcher | Sprint 2 | ~40 pages |
| Part | Sprint 3 | 37 pages |

---

## Priority queue

### Tier 1 — High traffic, most users need these

#### Spreadsheet *(in progress)*

**Why:** Cross-workbench tool used in almost every parametric model to drive
dimensions. Very small surface area but the critical concepts (cell references,
aliases, linking to model properties, driving dimensions from a spreadsheet)
are poorly explained everywhere. High documentation ROI.

**Planned pages (~10):**
- `index.md` — workbench overview, the parametric master-model pattern
- `tools/index.md` — tool index
- `tools/create-spreadsheet.md` — New Spreadsheet command
- `tools/formulas-and-expressions.md` — formula syntax, functions, operators (deep dive)
- `tools/aliases.md` — alias system + Set Alias tool
- `tools/model-integration.md` — reading model properties, driving dimensions
- `tools/import-export.md` — Import/Export CSV
- `tools/merge-split.md` — Merge/Split cells
- `tools/cell-formatting.md` — Alignment, bold/italic/underline, colours
- `tools/bind-sheet.md` — Bind Sheet (sync ranges between spreadsheets)

---

#### FEM (Finite Element Analysis)

**Why:** Huge workbench (50+ objects/commands). No authoritative community
docs exist. Every engineer doing structural or thermal analysis needs this.
Complex multi-step setup (geometry prep → mesh → material → loads/BCs →
solver → results) that desperately needs step-by-step guidance. Will be a
full dedicated sprint.

**Estimated pages:** 50–70  
**Complexity:** Very high — requires understanding FreeCAD FEM object model,
CalculiX/Elmer solver interfaces, and mesh concepts.

---

#### TechDraw

**Why:** Produces 2-D engineering drawings from 3-D models. Every
manufacturing workflow ends here. Well-defined tool set (~30 commands).
High user demand, frequently referenced in community support.

**Estimated pages:** 25–35  
**Complexity:** Medium — tools are well-defined, but the "view projection"
and annotation systems need careful explanation.

---

### Tier 2 — Specialised but widely used

#### Mesh

**Why:** The import/repair pipeline for STL files feeds directly into Part.
Many users hit this immediately when importing 3-D print files or scanned
geometry. Relatively small tool set (~15 commands). Completes the mesh↔solid
pipeline already referenced in Part docs.

**Estimated pages:** 12–15  
**Complexity:** Low — tools are mostly single-purpose utilities.

---

#### Assembly (built-in, FreeCAD 1.0+)

**Why:** Brand new in FreeCAD 1.0; no community docs exist yet. Critical for
multi-part models. Growing user interest. The joint/constraint system needs
thorough explanation.

**Estimated pages:** 20–30  
**Complexity:** High — new system with limited prior art to reference.

---

#### Sheet Metal

**Why:** Popular add-on that ships with some FreeCAD distributions. K-factor,
bend tables, and unfold operations are poorly documented everywhere.

**Estimated pages:** 20–25  
**Complexity:** Medium.

---

### Tier 3 — Useful but narrower audience

| Workbench | Est. pages | Notes |
|-----------|-----------|-------|
| **Path (CAM)** | 60+ | Very large; CNC machining workflow; own dedicated sprint |
| **BIM / Arch** | 50+ | Architecture/BIM; different audience from mechanical users |
| **Surface** | 8–10 | Freeform surface filling/sewing; niche use |
| **Reverse Engineering** | 8–10 | Point cloud → mesh → solid; scanning workflows |
| **Fem Post** | 15–20 | Post-processing FEM results; companion to FEM sprint |

---

## Decision log

| Date | Decision |
|------|----------|
| 2026-05-28 | Proposal written after completing Part workbench sprint |
| 2026-05-28 | Spreadsheet workbench selected as next sprint (small, high cross-workbench value) |
