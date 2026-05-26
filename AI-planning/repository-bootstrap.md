# Proposal: Repository Bootstrap & First Workbench Documentation Sprint

**Status:** Draft  
**Depends on:** `tool-documentation-format.md` (the template proposal)  
**Scope:** Standing up the full repo skeleton, wiring GitHub Pages CI, and
writing a prompt that generates the complete Part Design workbench documentation.

---

## 1. Overview

This proposal has three parts:

1. **Repo bootstrap** — every file needed to go from an empty repo to a locally
   working, deployable MkDocs site
2. **GitHub Pages publishing** — a GitHub Actions workflow that redeploys on
   every push to `main`
3. **Part Design documentation prompt** — a carefully scoped prompt that an AI
   agent can execute to produce all tool pages for the first workbench

---

## 2. Repository bootstrap

### 2.1 Final file tree after bootstrap

```
freecad-docs/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Pages CI
├── docs/
│   ├── index.md                # Landing page
│   ├── assets/
│   │   └── freecad-logo.svg    # Or .png; placeholder until we have one
│   └── part-design/
│       ├── index.md            # Workbench overview
│       └── tools/              # (populated by the documentation sprint)
├── .gitignore
├── agents.md
├── AI-planning/
│   ├── tool-documentation-format.md
│   └── repository-bootstrap.md   (this file)
├── mkdocs.yml
└── requirements.txt
```

### 2.2 `requirements.txt`

Pin to a specific version to prevent silent breakage from upstream changes.
Check `../mkdocs-material/pyproject.toml` for the current release version when
creating this file.

```text
mkdocs>=1.6
mkdocs-material>=9.5
```

Add optional dependencies only when a specific plugin is enabled:

```text
# Uncomment when tags plugin is enabled:
# mkdocs-material[imaging]      # social cards + image optimisation

# Uncomment for math support:
# pymdown-extensions>=10
```

### 2.3 `mkdocs.yml`

The following is a full working config. It targets the OSS (non-Insiders) release
of Material for MkDocs. Every feature listed here was verified against
`../mkdocs-material/mkdocs.yml` and `../mkdocs-material/docs/`.

```yaml
# ──────────────────────────────────────────────────────────────
# Site metadata
# ──────────────────────────────────────────────────────────────
site_name: FreeCAD Documentation
site_url: https://robertmassaioli.github.io/freecad-docs/
site_description: >-
  Community-maintained, in-depth documentation for FreeCAD —
  covering Sketcher, Part Design, and Part workbenches.
site_author: Robert Massaioli
copyright: >-
  Content licensed under
  <a href="https://creativecommons.org/licenses/by-sa/4.0/">CC BY-SA 4.0</a>

# ──────────────────────────────────────────────────────────────
# Source repository (enables the edit-page pencil icon)
# ──────────────────────────────────────────────────────────────
repo_name: robertmassaioli/freecad-docs
repo_url: https://github.com/robertmassaioli/freecad-docs
edit_uri: edit/main/docs/

# ──────────────────────────────────────────────────────────────
# Theme
# ──────────────────────────────────────────────────────────────
theme:
  name: material
  features:
    # Code blocks
    - content.code.annotate
    - content.code.copy
    # Inline tooltips on links
    - content.tooltips
    # Navigation
    - navigation.footer         # prev / next page links
    - navigation.indexes        # section index pages (index.md per folder)
    - navigation.sections       # render top-level sections as groups in sidebar
    - navigation.tabs           # top-level nav as tabs
    - navigation.top            # back-to-top button
    - navigation.tracking       # anchor tracking in URL
    # Search
    - search.highlight
    - search.share
    - search.suggest
    # TOC
    - toc.follow                # auto-scroll TOC to current heading
  palette:
    - media: "(prefers-color-scheme)"
      toggle:
        icon: material/link
        name: Switch to light mode
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: deep orange     # FreeCAD's brand colour approximation
      accent: orange
      toggle:
        icon: material/toggle-switch
        name: Switch to dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: deep orange
      accent: orange
      toggle:
        icon: material/toggle-switch-off
        name: Switch to system preference
  font:
    text: Roboto
    code: Roboto Mono

# ──────────────────────────────────────────────────────────────
# Plugins  (OSS only — no Insiders-only plugins)
# ──────────────────────────────────────────────────────────────
plugins:
  - search:
      lang: en

# ──────────────────────────────────────────────────────────────
# Markdown extensions
# Reference: ../mkdocs-material/docs/reference/ for each feature
# ──────────────────────────────────────────────────────────────
markdown_extensions:
  # Python Markdown built-ins
  - abbr
  - admonition          # !!! note / warning / tip / etc.
  - attr_list           # {.class} on elements
  - def_list            # definition lists
  - footnotes
  - md_in_html
  - tables
  - toc:
      permalink: true

  # PyMdown Extensions
  - pymdownx.betterem
  - pymdownx.caret      # superscript  H^2^O
  - pymdownx.critic     # tracked changes
  - pymdownx.details    # collapsible admonitions
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.keys       # keyboard keys  ++ctrl+s++
  - pymdownx.mark       # highlights  ==text==
  - pymdownx.smartsymbols
  - pymdownx.snippets   # file includes
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:
      alternate_style: true   # required for content tabs to render correctly
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde      # subscript  H~2~O + ~~strikethrough~~

# ──────────────────────────────────────────────────────────────
# Navigation
# Add new workbench sections here as they are written.
# ──────────────────────────────────────────────────────────────
nav:
  - Home: index.md
  - Part Design:
    - part-design/index.md
    - Tools:
      # Populated during the documentation sprint — see bootstrap proposal
      - part-design/tools/index.md
  # Future workbenches (uncomment when pages exist):
  # - Sketcher:
  #   - sketcher/index.md
  # - Part:
  #   - part/index.md
```

> **Note:** `<github-username>` has been resolved to `robertmassaioli`
> (repo: https://github.com/robertmassaioli/freecad-docs,
> site: https://robertmassaioli.github.io/freecad-docs/).

### 2.4 `docs/index.md` (landing page skeleton)

```markdown
# FreeCAD Documentation

Welcome to the community-maintained FreeCAD reference documentation.

This site provides in-depth coverage of FreeCAD's workbenches, tools,
and Python scripting API — written for clarity and progressively ordered
from beginner concepts to advanced techniques.

## Available workbenches

<div class="grid cards" markdown>

-   :material-cube-outline: **Part Design**

    ---

    Feature-based parametric solid modelling.  
    Create bodies, pads, pockets, patterns, and more.

    [:octicons-arrow-right-24: Part Design](part-design/index.md)

</div>

!!! info "Work in progress"
    Documentation is being written incrementally. Pages for Sketcher and
    Part are coming next.
```

### 2.5 `docs/part-design/index.md` (workbench overview skeleton)

```markdown
# Part Design Workbench

The **Part Design** workbench is FreeCAD's primary tool for creating
parametric solid models using a feature-based workflow. You build a model
by applying an ordered sequence of operations — each one recorded in the
model tree — so that changing an early dimension automatically updates
everything downstream.

## Key concepts

- **Body** — the single solid container every feature lives inside
- **Feature tree** — the ordered history of operations; the final shape
  is the result of applying them all in sequence
- **Tip** — the currently active (last) feature in the tree; determines
  what shape is visible

## Tool groups

| Group | What it does |
|-------|-------------|
| [Structure](tools/index.md#structure) | Body, datum planes/lines/points, sketch attachment |
| [Additive features](tools/index.md#additive) | Add material: Pad, Revolution, Loft, Pipe, Helix |
| [Subtractive features](tools/index.md#subtractive) | Remove material: Pocket, Groove, Hole, … |
| [Additive primitives](tools/index.md#primitives) | Insert solid primitives: Box, Cylinder, Sphere, … |
| [Transformations](tools/index.md#transformations) | Pattern and mirror features |
| [Dress-up](tools/index.md#dressup) | Fillet, Chamfer, Draft, Thickness |
| [Boolean](tools/index.md#boolean) | Combine or subtract bodies |
```

---

## 3. GitHub Pages publishing

### 3.1 Strategy

Use the **GitHub Actions + `gh-pages` branch** method. On every push to `main`,
Actions builds the site with `mkdocs build` and force-pushes the output to the
`gh-pages` branch. GitHub Pages then serves that branch at
`https://<username>.github.io/freecad-docs/`.

This is the approach documented in
`../mkdocs-material/docs/publishing-your-site.md` and is the most reliable
option for a public repository with no Insiders features.

### 3.2 `.github/workflows/deploy.yml`

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main
  # Allow manual trigger from the Actions tab
  workflow_dispatch:

permissions:
  contents: write   # needed to push to gh-pages branch

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0          # full history — needed if git-revision-date plugin is ever added

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Cache pip dependencies
        uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ hashFiles('requirements.txt') }}
          path: ~/.cache/pip

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Build and deploy
        run: mkdocs gh-deploy --force
```

### 3.3 First-deploy checklist

Before merging any PR that adds `deploy.yml`:

- [ ] The GitHub repo has been created and `main` is the default branch
- [ ] `mkdocs.yml` has the real `site_url` and `repo_url` (no `<placeholder>`)
- [ ] GitHub Pages is enabled in Settings → Pages → Source: **Deploy from branch**,
      Branch: `gh-pages`, Folder: `/ (root)`
- [ ] A smoke-test of `mkdocs build` passes locally with no errors
- [ ] `docs/index.md` exists (a build with zero pages will fail)

---

## 4. Part Design documentation sprint prompt

### 4.1 What the prompt needs to achieve

The prompt will be handed to an AI agent (Claude Code or equivalent) with access
to:
- This repository (`freecad-docs/`)
- The FreeCAD source at `../FreeCAD/`
- The Material for MkDocs source at `../mkdocs-material/`

It needs to produce:
1. A complete Markdown file for **every Part Design tool** following the format
   in `AI-planning/tool-documentation-format.md`
2. A `docs/part-design/tools/index.md` that acts as a master index with one-line
   descriptions of every tool, grouped by category
3. Updated `nav:` entries in `mkdocs.yml` for every new page

### 4.2 Complete tool inventory

The agent must document **all** of the following tools. This list was derived
from `../FreeCAD/src/Mod/PartDesign/Gui/Workbench.cpp`. Group membership is the
canonical grouping; use it for the tools/index.md table and for mkdocs.yml nav.

**Structure & setup**
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_Body` | `body.md` | Create a new Body container |
| `PartDesign_NewSketch` | `new-sketch.md` | Create a sketch attached to the Body |
| `PartDesign_CoordinateSystem` | `coordinate-system.md` | Insert a local coordinate system |
| `PartDesign_Plane` | `datum-plane.md` | Insert a datum plane |
| `PartDesign_Line` | `datum-line.md` | Insert a datum line |
| `PartDesign_Point` | `datum-point.md` | Insert a datum point |
| `PartDesign_ShapeBinder` | `shape-binder.md` | Reference external geometry into the Body |
| `PartDesign_SubShapeBinder` | `sub-shape-binder.md` | Reference a sub-element from another Body |
| `PartDesign_Clone` | `clone.md` | Clone a body as a linked copy |

**Additive features**
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_Pad` | `pad.md` | Extrude a sketch into a solid |
| `PartDesign_Revolution` | `revolution.md` | Revolve a sketch around an axis |
| `PartDesign_AdditiveLoft` | `additive-loft.md` | Loft through multiple sketches (additive) |
| `PartDesign_AdditivePipe` | `additive-pipe.md` | Sweep a profile along a path (additive) |
| `PartDesign_AdditiveHelix` | `additive-helix.md` | Sweep a profile along a helix (additive) |

**Additive primitives** (`PartDesign_CompPrimitiveAdditive`)
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_AdditiveBox` | `additive-box.md` | Add a box primitive |
| `PartDesign_AdditiveCylinder` | `additive-cylinder.md` | Add a cylinder primitive |
| `PartDesign_AdditiveSphere` | `additive-sphere.md` | Add a sphere primitive |
| `PartDesign_AdditiveCone` | `additive-cone.md` | Add a cone primitive |
| `PartDesign_AdditiveEllipsoid` | `additive-ellipsoid.md` | Add an ellipsoid primitive |
| `PartDesign_AdditiveTorus` | `additive-torus.md` | Add a torus primitive |
| `PartDesign_AdditivePrism` | `additive-prism.md` | Add a prism primitive |
| `PartDesign_AdditiveWedge` | `additive-wedge.md` | Add a wedge primitive |

**Subtractive features**
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_Pocket` | `pocket.md` | Extrude a cut into the solid |
| `PartDesign_Groove` | `groove.md` | Revolve a cut around an axis |
| `PartDesign_Hole` | `hole.md` | Create an engineered hole (threaded/counterbored/countersunk) |
| `PartDesign_SubtractiveLoft` | `subtractive-loft.md` | Loft a cut through multiple sketches |
| `PartDesign_SubtractivePipe` | `subtractive-pipe.md` | Sweep a cut along a path |
| `PartDesign_SubtractiveHelix` | `subtractive-helix.md` | Sweep a cut along a helix |

**Subtractive primitives** (`PartDesign_CompPrimitiveSubtractive`)
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_SubtractiveBox` | `subtractive-box.md` | Subtract a box primitive |
| `PartDesign_SubtractiveCylinder` | `subtractive-cylinder.md` | Subtract a cylinder primitive |
| `PartDesign_SubtractiveSphere` | `subtractive-sphere.md` | Subtract a sphere primitive |
| `PartDesign_SubtractiveCone` | `subtractive-cone.md` | Subtract a cone primitive |
| `PartDesign_SubtractiveEllipsoid` | `subtractive-ellipsoid.md` | Subtract an ellipsoid primitive |
| `PartDesign_SubtractiveTorus` | `subtractive-torus.md` | Subtract a torus primitive |
| `PartDesign_SubtractivePrism` | `subtractive-prism.md` | Subtract a prism primitive |
| `PartDesign_SubtractiveWedge` | `subtractive-wedge.md` | Subtract a wedge primitive |

**Transformation features**
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_Mirrored` | `mirrored.md` | Mirror a feature across a plane |
| `PartDesign_LinearPattern` | `linear-pattern.md` | Repeat a feature in a line |
| `PartDesign_PolarPattern` | `polar-pattern.md` | Repeat a feature in a circle |
| `PartDesign_MultiTransform` | `multi-transform.md` | Combine multiple transformations |

**Dress-up features**
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_Fillet` | `fillet.md` | Round selected edges |
| `PartDesign_Chamfer` | `chamfer.md` | Bevel selected edges |
| `PartDesign_Draft` | `draft.md` | Taper faces by a draft angle |
| `PartDesign_Thickness` | `thickness.md` | Hollow a solid to a uniform wall thickness |

**Boolean & special**
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_Boolean` | `boolean.md` | Boolean union/cut/common between bodies |
| `PartDesign_InvoluteGear` | `involute-gear.md` | Generate an involute gear profile |
| `PartDesign_Sprocket` | `sprocket.md` | Generate a sprocket profile |
| `PartDesign_WizardShaft` | `wizard-shaft.md` | Guided shaft design wizard |

**Tree management** (short pages — these are UI operations, not modelling features)
| Command | Slug | Description |
|---------|------|-------------|
| `PartDesign_MoveTip` | `move-tip.md` | Set the active tip feature |
| `PartDesign_MoveFeature` | `move-feature.md` | Move a feature to a different Body |
| `PartDesign_MoveFeatureInTree` | `move-feature-in-tree.md` | Reorder a feature in the tree |
| `PartDesign_DuplicateSelection` | `duplicate-selection.md` | Duplicate selected features |

**Total: 46 tool pages + 1 index page**

### 4.3 The prompt

Copy the block below verbatim as the agent's starting prompt. Fill in the
`<REPO_ROOT>` placeholder with the absolute path to this repository before
running.

---

```
You are writing the Part Design workbench section of a FreeCAD documentation
site. The site uses Material for MkDocs.

## Your working directories

- Documentation repo:   <REPO_ROOT>/
- FreeCAD source:       <REPO_ROOT>/../FreeCAD/
- Material for MkDocs:  <REPO_ROOT>/../mkdocs-material/

## Mandatory reading before you write anything

1. Read <REPO_ROOT>/AI-planning/tool-documentation-format.md in full.
   Every page you write MUST follow that template exactly — same sections,
   same order, same progressive-complexity principle.

2. Read <REPO_ROOT>/AI-planning/repository-bootstrap.md sections 4.2
   (the tool inventory and slug table) to know the full list of tools and
   their target file names.

3. Check the FreeCAD source for every tool before writing about it.
   IMPORTANT: all source lookups must be based on tag 1.1.1 — the
   authoritative FreeCAD version this documentation targets. Before
   reading any source file, confirm the working tree is at that tag:
     git -C ../FreeCAD describe --tags  # should output 1.1.1
   If it is not, check out the tag before proceeding:
     git -C ../FreeCAD checkout 1.1.1
   Key source paths:
   - Command implementations: ../FreeCAD/src/Mod/PartDesign/Gui/Command*.cpp
   - App-layer feature code:  ../FreeCAD/src/Mod/PartDesign/App/
   - Property names / types:  search for PROPERTY_ADD or App::Property in
     the .h files under ../FreeCAD/src/Mod/PartDesign/App/
   - UI strings / tooltips:   grep for QT_TRANSLATE_NOOP in the Gui/ files

4. Check Material for MkDocs syntax against:
   ../mkdocs-material/docs/reference/admonitions.md
   ../mkdocs-material/docs/reference/content-tabs.md
   ../mkdocs-material/docs/reference/code-blocks.md

## What to produce

### A. Tool pages

Write one Markdown file per tool into docs/part-design/tools/<slug>.md
using the slugs listed in AI-planning/repository-bootstrap.md §4.2.

Every page must contain all required sections from the template:
- One-sentence summary (verb-first, ≤20 words, no jargon)
- Overview (workbench, menu path, toolbar, shortcut if any)
- Intuition (analogy / mental model, 100–300 words)
- When to use it (3–6 scenario bullets)
- When NOT to use it (2–5 bullets with named alternatives)
- Step-by-step walkthrough (numbered, one action per step)
- Parameters (complete table, every UI control, in UI order)
- Common mistakes and pitfalls (≥2 !!! warning blocks: symptom / cause / fix)
- Python API (minimal runnable example + method signature table)
- See also (3–8 links to related tools)

Mark any Python API detail you cannot verify from the source with:
  <!-- TODO: verify against ../FreeCAD/src/Mod/PartDesign/App/ -->

Tree management tools (MoveTip, MoveFeature, MoveFeatureInTree,
DuplicateSelection) may omit the Python API section if no scripting
API is exposed — note the omission explicitly.

### B. Tools index page

Write docs/part-design/tools/index.md as a master reference page.
It must contain:
- A short intro paragraph (2–3 sentences)
- One table per tool group (matching the groups in bootstrap proposal §4.2)
- Each table row: tool name (linked to its page) | one-sentence description
- Named anchors on each table heading so mkdocs.yml section links work:
  {#structure}, {#additive}, {#subtractive}, {#primitives},
  {#transformations}, {#dressup}, {#boolean}

### C. Updated mkdocs.yml nav

Append the complete Part Design nav block to mkdocs.yml, replacing the
placeholder stub. Use the group structure from the bootstrap proposal.
The nav must list every tool page exactly once.

## Quality gates

Before finishing, verify:
1. Every file in the slug table has been created
2. mkdocs build runs with zero errors (run it from <REPO_ROOT>)
3. No internal link in any page 404s
4. Every Parameters table row corresponds to a real control visible in the
   FreeCAD task panel (verify against the source if you cannot run FreeCAD)
5. The progressive-complexity rule holds on every page — a reader stopping
   at the Intuition section must not have encountered a step-by-step
   instruction yet

## Execution order (recommended)

Work in this order to keep mkdocs.yml valid throughout:
1. Write the tools index page stub (just headings, no links yet)
2. Write tool pages group by group, starting with Structure & Setup,
   then Additive, then Subtractive, then Primitives, then Transformations,
   then Dress-up, then Boolean & Special, then Tree Management
3. Fill in the tools index page links as you complete each group
4. Update mkdocs.yml nav entries as you complete each group
5. Run mkdocs build after every group to catch errors early

## Do not

- Do not invent parameter names, default values, or method signatures.
  If you cannot find something in the source, use a TODO comment.
- Do not skip the "When NOT to use it" section — it is required.
- Do not write the Python API section from memory. Always grep the source.
- Do not create any files outside docs/part-design/tools/ or edits to
  mkdocs.yml. Do not touch docs/index.md or docs/part-design/index.md.
```

---

### 4.4 Guidance for running the prompt

- **Model choice:** Use the most capable available model (currently Claude
  Opus). This is a large writing task with many files; do not use a smaller
  model to save cost — accuracy against the source is more important than
  speed.
- **Session strategy:** The full 46-tool inventory is large. The execution
  order in the prompt (group by group, building the index and nav as you go)
  is designed to let the task be interrupted and resumed — each group is a
  coherent checkpoint. If the session context fills before all groups are
  done, start a new session with the same prompt and add:
  > "Groups already completed: [list]. Continue from [next group]."
- **Verification pass:** After the agent finishes, run a separate short
  prompt:
  > "Run `mkdocs build` in `<REPO_ROOT>`. List every warning and error.
  > For each broken internal link, fix it. For each TODO comment in a
  > Python API section, attempt to resolve it from the FreeCAD source."

---

## 5. Decisions (resolved)

| # | Decision | Resolution |
|---|----------|-----------|
| 1 | GitHub username / org | **`robertmassaioli`** — site at `https://robertmassaioli.github.io/freecad-docs/` |
| 2 | CC licence vs full copyright | **CC BY-SA 4.0** — consistent with FreeCAD wiki |
| 3 | Screenshots policy | **No screenshots for now.** May add later only if a fully automated capture pipeline can be built. Manual screenshots are permanently excluded. |
| 4 | OSS Material vs Insiders | **OSS only** — see `site-generator-decision.md` |
| 5 | FreeCAD version baseline | **1.1** (specifically tag `1.1.1`, the latest 1.1 patch release). Future versions documented in separate sessions. Source at `../FreeCAD/` should be checked out at tag `1.1.1` when sourcing facts. |
