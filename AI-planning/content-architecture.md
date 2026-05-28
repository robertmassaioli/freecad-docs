# Content Architecture

**Status:** Authoritative — all contributors and AI agents must follow this document  
**Scope:** Every page in the `docs/` tree and every entry in `mkdocs.yml`

---

## 1. The cardinal rule

> **One FreeCAD tool = one Markdown file = one left-hand-nav entry.**

This is the single most important structural constraint on this documentation site.
It is not negotiable, and it must never be traded away for the sake of speed or
convenience. Every tool that appears in FreeCAD's menus or toolbars deserves its
own dedicated page, its own permanent URL, and its own directly clickable entry in
the sidebar navigation.

### Why this matters

| Concern | Merged files (bad) | One-file-per-tool (correct) |
|---------|-------------------|----------------------------|
| Deep linking | Fragment URLs (`#section-name`) that break when prose is reordered | Stable, permanent per-tool URL |
| Navigation | Users must scroll a long page to find one tool | Every tool is a direct sidebar click |
| Searchability | Search results point to a wall-of-text page | Search lands on the exact tool page |
| Maintenance | A change to one tool's content disrupts the whole file | Edits are isolated to a single file |
| Content depth | Merged files create pressure to write shallow coverage of each tool | One file per tool enables full coverage per `tool-documentation-format.md` |
| Progressive documentation | Hard to add depth to a single tool without unbalancing a merged file | Any tool page can grow independently |
| AI generation quality | AI tools under-document each tool because a combined file hits length limits | AI tools can follow the full template for each tool |

---

## 2. Definitions

### What is a "tool" for this purpose?

A tool is any FreeCAD feature that has **any one** of:

- Its own menu entry (e.g. **Sketch → Sketcher geometries → Point**)
- Its own toolbar button
- Its own Python command class (e.g. `SketcherCreatePoint`, `PartDesign_Pad`)
- Its own task panel or dialog that opens when activated

If it has a dedicated name in the FreeCAD UI and does one thing, it gets one page.

### What is NOT a separate tool page?

- **Workbench index pages** — `docs/<workbench>/index.md` — overview of the workbench
- **Tools index pages** — `docs/<workbench>/tools/index.md` — navigation table linking to individual tool pages
- **Concept pages** — `docs/<workbench>/concepts/<slug>.md` — explanations of underlying ideas (e.g. attachment, the Body model), not tied to a single tool

---

## 3. Directory structure

```
docs/
└── <workbench-slug>/
    ├── index.md                    ← Workbench overview (required)
    ├── concepts/
    │   └── <concept-slug>.md       ← Conceptual deep-dives (optional)
    └── tools/
        ├── index.md                ← Navigation table for all tools (required)
        └── <tool-slug>.md          ← One file per tool (required for every tool)
```

### Workbench slugs

| Workbench | Directory slug |
|-----------|---------------|
| Part Design | `part-design` |
| Sketcher | `sketcher` |
| Part | `part` |
| Spreadsheet | `spreadsheet` |
| FEM | `fem` |
| TechDraw | `techdraw` |
| Assembly | `assembly` |
| Draft | `draft` |
| Mesh | `mesh` |
| Surface | `surface` |
| CAM / Path | `cam` |
| BIM | `bim` |
| Robot | `robot` |
| Points | `points` |
| Inspection | `inspection` |
| Reverse Engineering | `reverse-engineering` |
| OpenSCAD | `openscad` |
| Measure | `measure` |
| Curves Workbench | `curves-wb` |

### Tool slugs

- Lowercase, hyphen-separated
- Match the tool's FreeCAD UI name as closely as possible
- Drop workbench prefixes where they appear in the UI name  
  - `Draft Line` → `draft/tools/line.md`
  - `Sketcher Coincident Constraint` → `sketcher/tools/constraint-coincident.md`
  - `PartDesign Pad` → `part-design/tools/pad.md`

---

## 4. Navigation (mkdocs.yml)

Every tool must appear as a **separate, named entry** in the `nav:` block of
`mkdocs.yml`. Each entry must point to its own individual file.

### Correct nav pattern

```yaml
- Draft:
    - draft/index.md
    - Tools:
      - draft/tools/index.md
      - Drafting:
        - Line: draft/tools/line.md
        - Wire (Polyline): draft/tools/wire.md
        - Fillet: draft/tools/fillet.md
        - Arc: draft/tools/arc.md
        - Arc by 3 Points: draft/tools/arc-3-points.md
        - Circle: draft/tools/circle.md
        - Ellipse: draft/tools/ellipse.md
        - Rectangle: draft/tools/rectangle.md
        - Polygon: draft/tools/polygon.md
        - B-Spline: draft/tools/bspline.md
        - Cubic Bézier: draft/tools/bezier-cubic.md
        - Bézier Curve: draft/tools/bezier.md
        - Point: draft/tools/point.md
        - Facebinder: draft/tools/facebinder.md
        - ShapeString: draft/tools/shapestring.md
        - Hatch: draft/tools/hatch.md
```

### Incorrect nav pattern ✗

```yaml
- Draft:
    - draft/index.md
    - Tools:
      - draft/tools/index.md
      - Drafting: draft/tools/drafting.md        ← WRONG: all 16 tools in one file
      - Annotation: draft/tools/annotation.md    ← WRONG: multiple tools in one file
      - Modification: draft/tools/modification.md
```

### Nav grouping

Nav sub-groups (the named headings like `Drafting:`, `Geometric Constraints:`,
`Dress-up:`) are purely organisational — they do not correspond to files. They
mirror the groupings FreeCAD uses in its own menus and toolbars. Individual tool
entries within a group each point to their own file.

---

## 5. Index pages

### Workbench index (`docs/<workbench>/index.md`)

Contains:
- What the workbench is for (2–4 sentences)
- Key concepts unique to this workbench
- When to use this workbench vs others
- Brief overview table of major tool categories (linking to each category heading
  in `tools/index.md`)

Does **not** contain individual tool reference content.

### Tools index (`docs/<workbench>/tools/index.md`)

A pure navigation page. Contains:
- One table (or a series of grouped tables) listing every tool by name
- Each tool name links to its individual tool page
- One-sentence description per tool

Does **not** contain step-by-step instructions, parameters, or Python API content.
That content belongs in the individual tool page.

### Example — Part Design tools/index.md (correct pattern)

```markdown
## Additive features

| Tool | Description |
|------|-------------|
| [Pad](pad.md) | Extrude a closed sketch perpendicular to its plane |
| [Revolution](revolution.md) | Revolve a sketch profile around an axis |
| [Additive Loft](additive-loft.md) | Blend through two or more profiles |
```

### Example — Anti-pattern (merged index)

```markdown
## Filling
Creates a smooth NURBS surface that fills a closed boundary region...
### Step-by-step
1. Select a closed boundary loop of edges...
### Python API
surf = doc.addObject("Surface::Filling", "Fill")   ← WRONG: tool content in a grouped file
```

---

## 6. Anti-patterns catalogue

The following patterns have appeared in this repository and must be corrected.
They arose when AI tools chose speed over correctness during initial sprint
generation. Every case listed here represents a **known violation** that must be
refactored.

### AP-1: The "All Tools" dump file

**Symptom:** A file named `tools.md` that contains documentation for every tool
in the workbench as `##`-level sections.

**Examples in this repo:**
- `docs/surface/tools/tools.md` — 6 tools (Filling, Fill Boundary Curves, Sections, Extend Face, Blend Curve, Curve on Mesh)
- `docs/robot/tools/tools.md` — all Robot workbench tools
- `docs/openscad/tools/tools.md` — all OpenSCAD workbench tools
- `docs/points/tools/tools.md` — all Points workbench tools

**Fix:** Split into one file per tool. Delete the merged file. Add individual
nav entries in `mkdocs.yml`.

---

### AP-2: The "Category" dump file

**Symptom:** A file whose name is a category label rather than a tool name, and
whose content documents multiple tools under that category.

**Examples in this repo:**
- `docs/draft/tools/drafting.md` — 16 drafting tools in one file
- `docs/draft/tools/modification.md` — multiple modification tools
- `docs/draft/tools/annotation.md` — multiple annotation tools
- `docs/draft/tools/arrays.md` — multiple array tools
- `docs/draft/tools/utilities.md` — multiple utility tools
- `docs/draft/tools/snapping.md` — all snap modes in one file
- `docs/cam/tools/operations.md` — all CAM operations in one file
- `docs/cam/tools/dressup.md` — all CAM dress-up operations
- `docs/cam/tools/toolbits.md` — all tool bit content
- `docs/bim/tools/structure.md` — multiple BIM structure tools
- `docs/bim/tools/elements.md` — multiple BIM element tools
- `docs/bim/tools/annotation.md` — multiple BIM annotation tools
- `docs/bim/tools/ifc.md` — multiple IFC management tools
- `docs/fem/tools/constraints-mechanical.md` — multiple FEM constraints
- `docs/fem/tools/constraints-thermal.md` — multiple FEM thermal constraints
- `docs/fem/tools/constraints-other.md` — multiple FEM constraints
- `docs/fem/tools/solvers-and-equations.md` — multiple solvers
- `docs/fem/tools/results.md` — multiple result tools
- `docs/techdraw/tools/views.md` — multiple TechDraw view tools
- `docs/techdraw/tools/dimensions.md` — multiple TechDraw dimension tools
- `docs/techdraw/tools/annotations.md` — multiple TechDraw annotation tools
- `docs/techdraw/tools/extensions.md` — all TechDraw extension pack tools
- `docs/assembly/tools/joints-kinematic.md` — multiple joint types
- `docs/assembly/tools/joints-geometric.md` — multiple joint types
- `docs/assembly/tools/joints-coupled.md` — multiple joint types
- `docs/mesh/tools/modify.md` — multiple mesh modification tools
- `docs/mesh/tools/analyse.md` — multiple mesh analysis tools
- `docs/mesh/tools/boolean-cutting.md` — multiple boolean/cutting tools
- `docs/mesh/tools/segmentation.md` — multiple segmentation tools
- `docs/mesh/tools/import-export.md` — multiple import/export tools
- `docs/curves-wb/tools/curves.md` — multiple curve tools
- `docs/curves-wb/tools/surfaces.md` — multiple surface tools
- `docs/curves-wb/tools/analysis.md` — multiple analysis tools

**Fix:** Same as AP-1 — split into one file per tool, update nav.

---

### AP-3: The merged index (content in an index file)

**Symptom:** A `tools/index.md` that contains tool documentation inline rather
than linking out to individual pages.

**Fix:** Move the tool content to individual pages. Reduce the index file to
a navigation table.

---

## 7. Compliance status

### Fully compliant workbenches

These workbenches correctly follow the one-file-per-tool rule:

| Workbench | Files | Notes |
|-----------|-------|-------|
| Part Design | ~40 tool files | Reference implementation |
| Sketcher | ~40 tool files | Reference implementation |
| Part | ~37 tool files + 4 to split (Sprint 1) | Mostly compliant |
| Spreadsheet | ~8 tool files | Compliant |
| Inspection | 2 tool files | Small workbench, compliant |
| Measure | 1 tool file | Compliant |
| Assembly | 18 tool files | Compliant after Sprint 0 of the assembly restructure |

### Non-compliant workbenches (require refactoring)

These workbenches currently use merged files and must be refactored. Each one is
a sprint-sized body of work. For the complete sprint-by-sprint plan including
exact tool lists and target file names, see
[workbench-restructure-plan.md](workbench-restructure-plan.md).

| Workbench | Merged files | Exact tool count | Anti-pattern | Priority |
|-----------|-------------|-----------------|--------------|----------|
| Points | 1 | 6 | AP-1 | Sprint 0 |
| Surface | 1 | 6 | AP-1 | Sprint 0 |
| Reverse Engineering | 3 | 12 | AP-2 | Sprint 0 |
| OpenSCAD | 1 | 14 | AP-1 | Sprint 0 |
| Robot | 1 | 15 | AP-1 | Sprint 0 |
| Part | 4 | 15 | AP-2 | Sprint 1 |
| Mesh | 5 | 33 | AP-2 | Sprint 2 |
| CAM | 4 | 35 | AP-2 | Sprint 3 |
| BIM | 4 | 38 | AP-2 | Sprint 4 |
| FEM | 9 | ~51 | AP-2 | Sprint 5 |
| Draft | 6 | ~64 + 16 snap modes | AP-2 | Sprint 6 |
| TechDraw | 6 | ~77 | AP-2 | Sprint 7 |
| Curves WB | 3 | 37 | AP-2 | Sprint 8 |

---

## 8. Refactoring procedure

When splitting a merged file into individual tool files, follow these steps in
order. Do the complete workbench in one commit — do not leave a partially split
workbench in the repository.

1. **Identify every tool** in the merged file — each `##`-level section is one
   tool.

2. **Create one new file** per tool at `docs/<workbench>/tools/<tool-slug>.md`.
   Copy the tool's content into that file.

3. **Apply the full tool template** from `AI-planning/tool-documentation-format.md`
   to each new file. Fill in any sections that are missing. The merged files
   typically lack: one-sentence summary, Intuition section, When to use, When NOT
   to use, and full Parameters table. These must be written for each tool.

4. **Update `docs/<workbench>/tools/index.md`** to be a pure navigation table
   linking to the new individual files. Delete any tool content that was inline
   in the index.

5. **Update `mkdocs.yml`** to replace each merged-file nav entry with individual
   tool entries. Follow the nav grouping conventions of the FreeCAD UI for that
   workbench.

6. **Delete the merged file.**

7. **Run `mkdocs build --strict`** and confirm zero warnings. All internal links
   must resolve.

---

## 9. Concepts pages

Some workbenches have concepts that underpin multiple tools and deserve dedicated
explanatory pages that are not tool reference pages.

**Correct use of concept pages:**
- `part-design/concepts/body-and-origin.md` — explains the Body model, origin
  planes, and the feature tree; not tied to one tool
- `part-design/concepts/attachment.md` — explains the attachment system;
  referenced from many tool pages

**Incorrect use of concept pages:**
- Using a concepts page as a dumping ground for tool content that should be in
  individual tool pages

Concept pages live at `docs/<workbench>/concepts/<slug>.md` and appear in the nav
under a `Concepts:` subsection before `Tools:`.

---

## 10. The tools/index.md and its role in navigation

The `tools/index.md` page is a **pure navigation hub**. Its only job is to give
users a scannable overview of every tool in the workbench and let them click to
the right page. It must never contain:

- Step-by-step instructions (those belong in the tool page)
- Parameters reference (tool page)
- Python API content (tool page)
- Prose that duplicates the tool page's Overview section

It **should** contain:
- A one-paragraph intro of 2–3 sentences about this workbench's tool set
- One table (or grouped tables) with every tool, linking to its individual page
- A one-sentence description per tool (not the full Overview, just the summary)

This keeps the index fast to scan and prevents it from drifting out of sync with
the individual pages.

---

## 11. Content format for individual tool pages

Every individual tool page must follow the template defined in
`AI-planning/tool-documentation-format.md`. That document governs the internal
structure of each tool page (sections, order, length targets, deep-dive rules,
Python API format, etc.).

This Content Architecture document governs **where pages live** and **how the nav
is structured**. The Tool Documentation Format document governs **what goes inside
each page**.

Both documents are required reading before writing any page on this site.

---

## 12. Screenshot and media policy

- **No manual screenshots.** Screenshots age badly across FreeCAD versions and
  cannot be regenerated cheaply. Do not add `![…](…)` image references to any
  page until an automated capture pipeline exists.
- **Diagrams** using Mermaid (```` ```mermaid ```` fenced blocks) are acceptable
  for flow diagrams and decision trees. They render in OSS Material for MkDocs.
- **Video embeds** are a future option for complex tools; deferred until the
  pipeline is defined.

---

## 13. Cross-workbench links

When linking from one workbench's page to another:

- Use **relative paths**: `../../part/tools/boolean-cut.md` not absolute URLs
- Run `mkdocs build --strict` after adding cross-workbench links — it catches
  all broken internal links as build errors
- Track unresolved links in `AI-planning/pending-cross-links.md` if the target
  page does not yet exist

---

## 14. Navigation architecture — solving the crowded header

### 14.1 The problem

`navigation.tabs` makes every **top-level** `nav:` entry a horizontal tab. With
20+ workbenches defined at the top level, the header tab bar overflows and becomes
unusable. The root cause is not the number of workbenches — it is the flat nav
structure where every workbench is a peer of "Home".

### 14.2 The solution: two-tab structure with a workbench hub

Collapse all workbench entries into a single second-level group so only two tabs
appear:

| Tab | Page | Purpose |
|-----|------|---------|
| **Home** | `docs/index.md` | Site intro, what this documentation covers |
| **Workbenches** | `docs/workbenches/index.md` | Visual workbench picker hub + full per-workbench sidebar nav |

Within the Workbenches tab, each workbench appears as a labelled **section** in
the sidebar (powered by `navigation.sections`). Only the currently active
workbench section is expanded in full — all others collapse to a single link to
their index page (powered by `navigation.prune`). Breadcrumbs (powered by
`navigation.path`) orient the user within the deep tool hierarchy.

### 14.3 Required mkdocs.yml changes

#### Features — remove

```yaml
# REMOVE this line (incompatible with navigation.prune):
- navigation.expand   # ← not currently present; do not add it
```

#### Features — add

```yaml
theme:
  features:
    # --- existing, keep ---
    - content.code.annotate
    - content.code.copy
    - content.tooltips
    - navigation.footer
    - navigation.indexes
    - navigation.sections
    - navigation.tabs
    - navigation.top
    - navigation.tracking
    - search.highlight
    - search.share
    - search.suggest
    - toc.follow

    # --- new additions ---
    - navigation.tabs.sticky   # tabs stay pinned below header when scrolling
    - navigation.prune         # sidebar shows only current workbench (experimental, OSS ≥9.2)
    - navigation.path          # breadcrumbs above page title (experimental, OSS ≥9.7)
```

All three additions are in the free/OSS release of Material for MkDocs. None
requires Insiders. The `experimental` flag means the API could change in a future
minor version — acceptable given the repo's pinned `>=9.7,<10.0` requirement.

#### Nav structure — before (20+ tabs)

```yaml
nav:
  - Home: index.md
  - Part Design:
      - part-design/index.md
      - ...
  - Sketcher:
      - sketcher/index.md
      - ...
  - Part:                     # ← tab 3 of 20+
  ...
```

#### Nav structure — after (2 tabs)

```yaml
nav:
  - Home: index.md
  - Workbenches:
      - workbenches/index.md         # hub page (new file)
      # ── Core Modelling ──────────────────────────────────────────
      - Part Design:
          - part-design/index.md
          - Concepts:
              - ...
          - Tools:
              - part-design/tools/index.md
              - ...
      - Sketcher:
          - sketcher/index.md
          - Tools:
              - sketcher/tools/index.md
              - ...
      - Part:
          - part/index.md
          - Tools:
              - part/tools/index.md
              - ...
      - Spreadsheet:
          - spreadsheet/index.md
          - Tools:
              - spreadsheet/tools/index.md
              - ...
      # ── Engineering & Analysis ───────────────────────────────────
      - FEM:
          - ...
      - Assembly:
          - ...
      - Measure:
          - ...
      - Inspection:
          - ...
      # ── Drawing & Documentation ──────────────────────────────────
      - TechDraw:
          - ...
      - Draft:
          - ...
      - BIM:
          - ...
      # ── Advanced & Specialist ────────────────────────────────────
      - CAM:
          - ...
      - Surface:
          - ...
      - Mesh:
          - ...
      - Curves Workbench:
          - ...
      - Reverse Engineering:
          - ...
      - Robot:
          - ...
      - Points:
          - ...
      - OpenSCAD:
          - ...
```

### 14.4 The hub page: docs/workbenches/index.md

Create `docs/workbenches/index.md` as a visual picker page. It uses Material's
card grid feature (`attr_list` + `md_in_html` — both already enabled in
`mkdocs.yml`) to display workbenches grouped by domain. The page hides its
table of contents since it has no body sections to navigate:

```markdown
---
hide:
  - toc
---

# Workbenches

Choose a workbench below to explore its tools and concepts.
All content is accurate for FreeCAD 1.1.

---

## Core Modelling

<div class="grid cards" markdown>

-   :material-cube-outline:{ .lg .middle } __Part Design__

    ---

    Parametric solid modelling using the Body feature tree — Pad, Pocket,
    Loft, Hole, and every dress-up operation.

    [:octicons-arrow-right-24: Part Design](../part-design/index.md)

-   :material-pencil-ruler:{ .lg .middle } __Sketcher__

    ---

    Fully constrained 2-D profiles that drive Part Design features.
    Geometric and dimensional constraints, B-splines, external geometry.

    [:octicons-arrow-right-24: Sketcher](../sketcher/index.md)

-   :material-shape-outline:{ .lg .middle } __Part__

    ---

    Lower-level Boolean operations, primitives, sweeps, and lofts directly
    on OCCT solids — without the feature-tree model.

    [:octicons-arrow-right-24: Part](../part/index.md)

-   :material-table:{ .lg .middle } __Spreadsheet__

    ---

    Parametric master-model spreadsheets: cell aliases, formula expressions,
    driving model dimensions from a spreadsheet.

    [:octicons-arrow-right-24: Spreadsheet](../spreadsheet/index.md)

</div>

## Engineering & Analysis

<div class="grid cards" markdown>

-   :material-function-variant:{ .lg .middle } __FEM__

    ---

    Finite element analysis — mesh, material, loads, CalculiX/Elmer
    solvers, and post-processing.

    [:octicons-arrow-right-24: FEM](../fem/index.md)

-   :material-link-variant:{ .lg .middle } __Assembly__

    ---

    Multi-body assembly with kinematic, geometric, and coupled joints.
    Simulation and exploded views.

    [:octicons-arrow-right-24: Assembly](../assembly/index.md)

-   :material-ruler:{ .lg .middle } __Measure__

    ---

    Linear, angular, and radius measurements directly on 3-D geometry.

    [:octicons-arrow-right-24: Measure](../measure/index.md)

-   :material-magnify:{ .lg .middle } __Inspection__

    ---

    Visual inspection and element-level interrogation of solid geometry.

    [:octicons-arrow-right-24: Inspection](../inspection/index.md)

</div>

## Drawing & Documentation

<div class="grid cards" markdown>

-   :material-drawing:{ .lg .middle } __TechDraw__

    ---

    2-D engineering drawings from 3-D models — views, dimensions,
    annotations, hatching, and the extension pack.

    [:octicons-arrow-right-24: TechDraw](../techdraw/index.md)

-   :material-vector-line:{ .lg .middle } __Draft__

    ---

    2-D freehand drawing, annotations, arrays, and snapping tools.
    Geometry lives in 3-D space on a configurable working plane.

    [:octicons-arrow-right-24: Draft](../draft/index.md)

-   :material-city:{ .lg .middle } __BIM__

    ---

    Building Information Modelling — IFC elements, structure hierarchy,
    annotations, and IFC export.

    [:octicons-arrow-right-24: BIM](../bim/index.md)

</div>

## Advanced & Specialist

<div class="grid cards" markdown>

-   :material-cog:{ .lg .middle } __CAM__

    ---

    Computer-aided manufacturing — job setup, machining operations,
    dress-ups, and tool bit libraries.

    [:octicons-arrow-right-24: CAM](../cam/index.md)

-   :material-wave:{ .lg .middle } __Surface__

    ---

    NURBS surface modelling — filling, sections, blend curves, extend,
    and curve-on-mesh.

    [:octicons-arrow-right-24: Surface](../surface/index.md)

-   :material-grid:{ .lg .middle } __Mesh__

    ---

    Import, repair, analyse, and convert STL and other mesh formats.

    [:octicons-arrow-right-24: Mesh](../mesh/index.md)

-   :material-chart-bell-curve:{ .lg .middle } __Curves Workbench__

    ---

    Advanced curve and surface tools beyond the built-in workbenches.

    [:octicons-arrow-right-24: Curves Workbench](../curves-wb/index.md)

-   :material-rotate-3d:{ .lg .middle } __Reverse Engineering__

    ---

    Point cloud and mesh reconstruction into solid geometry.

    [:octicons-arrow-right-24: Reverse Engineering](../reverse-engineering/index.md)

-   :material-robot:{ .lg .middle } __Robot__

    ---

    Robot arm simulation and trajectory planning.

    [:octicons-arrow-right-24: Robot](../robot/index.md)

-   :material-dots-grid:{ .lg .middle } __Points__

    ---

    Point cloud import and display.

    [:octicons-arrow-right-24: Points](../points/index.md)

-   :material-code-braces:{ .lg .middle } __OpenSCAD__

    ---

    Import and interact with OpenSCAD geometry inside FreeCAD.

    [:octicons-arrow-right-24: OpenSCAD](../openscad/index.md)

</div>
```

### 14.5 How it feels to the user

| User action | Result |
|-------------|--------|
| Arrives at the site | Sees "Home" and "Workbenches" tabs — uncluttered |
| Clicks "Workbenches" tab | Hub page with categorised card grid — picks a workbench visually |
| Clicks "Part Design" card | Lands on Part Design overview; sidebar shows Part Design fully expanded; all other workbenches collapsed to single index-page links |
| Drills into a tool page | Breadcrumbs show: `Workbenches / Part Design / Tools / Pad`; sidebar still shows full Part Design tool tree |
| Wants a different workbench | Either clicks "Workbenches" tab to return to hub, or clicks one of the collapsed workbench links in the sidebar |
| Uses search | Search still finds pages across all workbenches; landing on a result re-expands the correct workbench in the sidebar |

### 14.6 Why not navigation.instant?

`navigation.instant` (XHR-based SPA navigation) is OSS and would make the site
feel faster, but it requires `site_url` to be set (already set) and has no
interaction issues with the other features above. It is a safe future addition but
deferred because it changes caching behaviour and has known edge cases with some
MkDocs plugins. Add it once the navigation restructure is stable.

```yaml
# Safe to add later — does NOT require Insiders:
- navigation.instant
- navigation.instant.progress    # progress bar on slow connections
```

Do NOT add `navigation.instant.prefetch` — that is Insiders-only.

### 14.7 What Insiders would add (but cannot be used)

For context: the `projects` plugin (Insiders-only) would allow each workbench to
be a fully independent sub-site with its own nav, isolated builds, and its own
header. This would be the ideal solution architecturally. It cannot be used.

The two-tab + hub + prune approach described above achieves roughly the same UX
goal within the OSS constraints.

### 14.8 Alternative: domain-grouped category tabs

If the two-tab structure feels too sparse (only "Home" and "Workbenches"), an
alternative is to group workbenches into 4–5 domain-specific top-level sections
that each become a tab:

| Tab | Workbenches |
|-----|------------|
| **Home** | `index.md` |
| **Core Modelling** | Part Design, Sketcher, Part, Spreadsheet |
| **Engineering** | FEM, Assembly, Measure, Inspection |
| **Drawing** | TechDraw, Draft, BIM |
| **Specialist** | CAM, Surface, Mesh, Curves WB, Reverse Engineering, Robot, Points, OpenSCAD |

This gives 5 tabs — still manageable, and the tab label communicates the domain.
The sidebar within each tab uses `navigation.sections` to separate workbenches,
and `navigation.prune` still applies.

**Drawback:** Workbench categorisation is subjective (is Assembly "Engineering" or
"Core Modelling"?). Users who have a workbench name in mind may not immediately
know which category tab to look in.

**Recommendation:** Start with the two-tab hub (§14.3). The category-tab approach
can be adopted later if user feedback shows the hub picker page is a friction point.

---

## Decision log

| Date | Decision |
|------|----------|
| 2026-05-28 | Document created to formally codify one-file-per-tool rule after multiple workbench sprints incorrectly merged tools into grouped files |
| 2026-05-28 | Added §14 navigation architecture proposal: two-tab hub structure with navigation.prune + navigation.path to solve crowded header |
