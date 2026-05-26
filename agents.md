# FreeCAD Docs — Agent Guide

## Project purpose

This repository produces a **static documentation site** for FreeCAD, built with
[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/). The initial
scope covers three tightly-related workbenches:

| Workbench | Description |
|-----------|-------------|
| **Sketcher** | 2-D constrained sketching — the foundation for all solid modelling |
| **Part Design** | Feature-based parametric solid modelling (Body/Pad/Pocket/…) |
| **Part** | Lower-level OCCT solid / Boolean operations workbench |

The long-term goal is to document FreeCAD in its entirety, but those three
workbenches are the first priority.

---

## Authoritative sources of truth

### FreeCAD source

The FreeCAD source code is checked out at **`../FreeCAD/`** (one directory up,
i.e. `/Users/robertmassaioli/Code/freecad-related-development/FreeCAD/`).  
Use it as the primary reference for:

- Command names, Python API calls, property names, and enumerations
- Module structure and how workbenches are registered
- Internal algorithms (when understanding them helps explain behaviour)

Key paths inside the source tree:

| Path | Contains |
|------|----------|
| `src/Mod/Sketcher/` | Sketcher C++ app layer + Python init |
| `src/Mod/Sketcher/Gui/` | Sketcher GUI commands and task panels |
| `src/Mod/PartDesign/` | PartDesign app layer |
| `src/Mod/PartDesign/Gui/` | PartDesign GUI commands |
| `src/Mod/Part/` | Part workbench |
| `src/Mod/Part/App/` | Part shapes, features, topology wrappers |
| `src/Mod/Part/Gui/` | Part GUI commands |
| `src/App/` | Core FreeCAD document model (`App::Document`, `App::Property*`) |
| `src/Gui/` | Main window, workbench registration, dock panels |

### Material for MkDocs source

The Material for MkDocs source is checked out at **`../mkdocs-material/`**
(i.e. `/Users/robertmassaioli/Code/freecad-related-development/mkdocs-material/`).
Use it as the authoritative reference for:

- Exactly which Markdown extensions and features are supported and how they are
  invoked (don't guess syntax from memory — check the source)
- Plugin configuration options (`material/plugins/`)
- Template structure for custom overrides (`material/templates/`, `material/overrides/`)
- The built-in reference docs under `docs/reference/` — these are the canonical
  examples of every supported Markdown feature

Key paths inside the Material for MkDocs checkout:

| Path | Contains |
|------|----------|
| `docs/reference/` | One page per supported Markdown feature (admonitions, code blocks, content tabs, grids, …) |
| `docs/setup/` | Configuration guides (navigation, search, tags, versioning, social cards, …) |
| `docs/plugins/` | Plugin reference (blog, search, tags, offline, …) |
| `material/templates/` | Jinja2 base templates (`base.html`, `main.html`, partials) |
| `material/extensions/` | Python Markdown extensions bundled with the theme |
| `material/plugins/` | Python plugin implementations |
| `mkdocs.yml` | The theme's own site config — the best real-world `mkdocs.yml` example |

---

## Site generator — Material for MkDocs (OSS release only)

**This project targets the free, open-source release of Material for MkDocs
exclusively. Do not use, recommend, or configure any Insiders-only feature.**

If you are unsure whether a feature is OSS or Insiders, check
`../mkdocs-material/docs/` — Insiders-only features are tagged with an
`<!-- md:flag insiders -->` marker. When in doubt, do not use it.

Key features that are **Insiders-only and must not be used**:
- Social cards (`social` plugin)
- Typeset plugin
- Projects plugin
- `navigation.instant.prefetch` (sub-feature of instant loading)

Key features that **are available in OSS** and should be used freely:
- Versioning via `mike` (version selector in the header)
- All PyMdown markdown extensions
- Full navigation system (tabs, sections, indexes, breadcrumbs)
- Built-in search with highlighting and suggestions
- Blog plugin
- Dark / light mode palette toggle

The site is built with `mkdocs` + the `material` theme.  
Configuration lives in **`mkdocs.yml`** at the repo root.

### Useful conventions

- Source docs live under **`docs/`**.
- Use **Markdown** (`.md`) for all pages.
- Admonitions, code blocks with syntax highlighting, and tabbed content are all
  supported via the Material theme — prefer them over plain prose where they aid
  clarity.
- Navigation is defined in `mkdocs.yml` under the `nav:` key; update it whenever
  a new page is added.
- Screenshots and diagrams go in `docs/assets/` (create subdirectories as needed).

### Suggested directory structure

```
docs/
├── index.md                      Landing / overview page
├── sketcher/
│   ├── index.md                  Sketcher overview
│   ├── concepts/
│   │   ├── constraints.md
│   │   ├── degrees-of-freedom.md
│   │   └── sketch-lifecycle.md
│   ├── tools/                    One page per tool / command group
│   │   ├── lines-and-arcs.md
│   │   ├── constraints-coincident.md
│   │   └── …
│   └── reference/
│       └── python-api.md
├── part-design/
│   ├── index.md
│   ├── concepts/
│   │   ├── body-and-origin.md
│   │   ├── feature-tree.md
│   │   └── tip.md
│   ├── features/                 Pad, Pocket, Revolution, Loft, …
│   │   └── …
│   └── reference/
│       └── python-api.md
└── part/
    ├── index.md
    ├── concepts/
    │   └── brep-topology.md
    ├── operations/               Boolean, Fillet, Chamfer, …
    │   └── …
    └── reference/
        └── python-api.md
```

---

## Writing guidelines

1. **Accuracy first** — every claim about behaviour, property names, or Python
   calls must be verifiable against the FreeCAD source in `../FreeCAD/` or a
   running FreeCAD session.  If uncertain, mark it with `<!-- TODO: verify -->`.
2. **Audience** — write for an intermediate user who understands 3-D CAD
   concepts but is new to FreeCAD's specific workflow.
3. **Python examples** — show the scripting API where it exists; copy exact
   method signatures from the source (search `src/Mod/*/App/*.cpp` for
   `PyObject`, or the corresponding `.py` wrappers).
4. **Cross-links** — link related pages liberally using relative paths
   (e.g. `[Pad](../part-design/features/pad.md)`).
5. **Admonition types to use**:
   - `!!! note` — extra context or clarification
   - `!!! tip` — workflow advice
   - `!!! warning` — common pitfall or data-loss risk
   - `!!! example` — worked examples with code
6. **Version notes** — FreeCAD 1.0+ (the first stable series) is the baseline.
   If behaviour changed between versions, use a `!!! info "Version X.Y"` block.

---

## How to find information in the source

### Looking up Material for MkDocs syntax

Always check `../mkdocs-material/docs/reference/` before writing or advising on
Markdown feature syntax — the pages there are the ground truth:

```
# Which admonition types are valid?
grep -r "type:" ../mkdocs-material/docs/reference/admonitions.md | head -20

# What content-tab syntax does the theme expect?
cat ../mkdocs-material/docs/reference/content-tabs.md

# What plugin options does the search plugin accept?
cat ../mkdocs-material/docs/setup/setting-up-site-search.md
```

### Finding what a command does

```
# List all command classes for a workbench
grep -r "class Cmd" ../FreeCAD/src/Mod/Sketcher/Gui/ --include="*.cpp" -l

# Find the Python name of a constraint
grep -r "ConstraintType" ../FreeCAD/src/Mod/Sketcher/App/ --include="*.cpp"
```

### Finding Python API methods

```
# Property / method names exposed to Python (look for Py:: wrappers)
grep -r "PyObject\|def " ../FreeCAD/src/Mod/PartDesign/App/ --include="*.cpp"

# FeaturePython scripts for advanced features
ls ../FreeCAD/src/Mod/PartDesign/Scripts/
```

### Finding UI strings / descriptions

```
# Tooltip / menu text (useful for matching source to UI labels)
grep -r "QT_TRANSLATE_NOOP\|tr(" ../FreeCAD/src/Mod/Sketcher/Gui/ --include="*.cpp"
```

---

## Development workflow

```bash
# Install dependencies (once)
pip install mkdocs-material

# Serve locally with live reload
mkdocs serve

# Build the static site
mkdocs build
```

The built site lands in `site/` (git-ignored).

---

## Out of scope (for now)

- FEM, CAM, BIM, Draft, TechDraw, and all other workbenches
- Addon/extension development (see `../freecad-todo-tree/` for that)
- FreeCAD internals / C++ API documentation
