# Proposal: Site Generator Decision — Material for MkDocs vs VitePress vs Docusaurus

**Status:** Draft — decision required before implementation begins  
**Triggered by:** Concern over Insiders paywalled features and versioning support

---

## 1. The question

The current bootstrap proposal assumes **Material for MkDocs (OSS release)**.
This document evaluates whether switching to **VitePress** or **Docusaurus**
would better serve the goals of this project — particularly versioning across
FreeCAD releases.

---

## 2. Key requirement: versioning

FreeCAD versions docs must need to track:

| FreeCAD version | Status |
|----------------|--------|
| 0.21 | Previous stable — some users still on it |
| 1.0 | Current stable — **our baseline** |
| 1.1 | In development — will need docs eventually |

Versioning means a reader can see docs for the FreeCAD version they actually
have installed, not just the latest. A version selector in the header is the
standard UI pattern.

---

## 3. Critical clarification: what is actually behind the Material for MkDocs paywall?

Before evaluating alternatives, it is worth being precise about what the
Insiders paywall actually gates. The following was verified directly against
`../mkdocs-material/docs/`:

### Insiders-only (not available in OSS):
| Feature | Impact on this project |
|---------|------------------------|
| Social cards (auto-generated OG images) | Nice-to-have; not needed for useful docs |
| Typeset plugin (custom TOC rendering) | Nice-to-have |
| Projects plugin (sub-site composition) | Not needed for a single docs site |
| Some `navigation.instant.prefetch` sub-features | Marginal UX improvement |

### Available in OSS (free):
| Feature | Notes |
|---------|-------|
| **Versioning** via `mike` | Full version selector in header, aliases (`latest`, `dev`) |
| All Markdown extensions | Admonitions, content tabs, code blocks, mermaid diagrams, etc. |
| Full navigation system | Tabs, sections, indexes, breadcrumbs, footer links |
| Search with highlighting and suggestions | |
| Blog plugin | OSS since Material 9.x |
| Dark / light mode |  |
| Edit-page links | |
| Git revision date (via plugin) | |

**The versioning feature is entirely free.** This is the most important finding:
the core capability that prompted this evaluation does not require Insiders.

---

## 4. Candidate comparison

### 4.1 Material for MkDocs (OSS)

| Dimension | Assessment |
|-----------|-----------|
| Versioning | ✅ Mature — `mike` plugin, version selector in header, aliases, "you are viewing an old version" banner |
| Markdown | ✅ Native Markdown + PyMdown Extensions; no JSX/MDX |
| Build toolchain | ✅ Pure Python: `pip install mkdocs-material` |
| GitHub Pages deploy | ✅ `mkdocs gh-deploy` one-liner; well-documented |
| FreeCAD ecosystem fit | ✅ FreeCAD is Python; contributors comfortable with Python tooling |
| Search | ✅ Client-side, fast, built-in |
| Ecosystem maturity | ✅ Very mature, actively maintained |
| Paywalled features needed? | ❌ None — see §3 above |
| Local source available? | ✅ `../mkdocs-material/` already checked out |

**Verdict:** All required features are in the OSS release. No paywalled features
are needed for this project.

---

### 4.2 VitePress

| Dimension | Assessment |
|-----------|-----------|
| Versioning | ⚠️ **Not built-in.** The standard approach is to manually deploy different versions to different sub-paths (`/v1.0/`, `/v1.1/`) and write a custom version switcher component in Vue. There is no first-class versioning CLI like `mike` or Docusaurus. |
| Markdown | ✅ Standard Markdown + custom Vue components in `.md` files |
| Build toolchain | ⚠️ Requires Node.js + npm; adds a separate runtime dependency |
| GitHub Pages deploy | ✅ Standard GitHub Actions + `npm run build` |
| FreeCAD ecosystem fit | ⚠️ Vue/Node.js is foreign to the FreeCAD/Python community |
| Search | ✅ MiniSearch, fast |
| Ecosystem maturity | ✅ Actively maintained by Evan You / Vue core team |
| Paywalled features? | ✅ Fully open-source, no paid tier |
| Local source available? | ❌ Would need to be added |

**Verdict:** VitePress has no paid tier, which was the stated concern — but it
**trades one problem for a bigger one**: the versioning story is immature and
manual. Implementing a clean per-FreeCAD-version docs system in VitePress would
require significant custom work. Not recommended.

---

### 4.3 Docusaurus

| Dimension | Assessment |
|-----------|-----------|
| Versioning | ✅ Best-in-class. `npx docusaurus docs:version 1.0` snapshots the current docs. Version selector, "you are viewing an old version" banner, and per-version sidebar nav all built in. |
| Markdown | ⚠️ MDX (Markdown + JSX). Simple pages work fine, but complex examples require React component knowledge |
| Build toolchain | ⚠️ Requires Node.js + npm; React dependency |
| GitHub Pages deploy | ✅ Well-documented |
| FreeCAD ecosystem fit | ⚠️ React/Node.js is foreign territory |
| Search | ⚠️ Algolia DocSearch (requires application/approval) or community Lunr plugin; no zero-config solution as good as Material's built-in |
| Ecosystem maturity | ✅ Very mature, Meta-backed, used by React, Jest, Babel, etc. |
| Paywalled features? | ✅ Fully open-source |
| Local source available? | ❌ Would need to be added |

**Verdict:** Docusaurus has the best versioning of the three, but it's meaningfully
heavier: Node.js runtime, React knowledge required, search needs external setup.
The versioning advantage over `mike` is marginal for a project that will have
maybe 2–3 tracked FreeCAD versions at most. Not recommended unless the team is
already comfortable with React.

---

## 5. Head-to-head on the versioning question specifically

| | Material for MkDocs + mike | VitePress | Docusaurus |
|---|---|---|---|
| Version snapshot command | `mike deploy 1.0` | Manual | `docusaurus docs:version 1.0` |
| Version selector UI | Built-in (OSS) | Manual Vue component | Built-in |
| "Old version" warning | Supported via theme override | Manual | Built-in |
| Keep old versions on gh-pages | ✅ `mike` handles this | Manual | ✅ Built-in |
| Effort to maintain 3 versions | Low | High | Low |

`mike` is a mature, battle-tested tool. It stores each version as a sub-directory
of the `gh-pages` branch and manages a `versions.json` manifest. The Material
theme reads this manifest and renders the version picker. This is not a second-
class solution — it is what the Material for MkDocs project itself recommends and
documents at length.

---

## 6. Recommendation

**Stay with Material for MkDocs (OSS).** The Insiders paywall gates no features
this project needs. Versioning via `mike` is available for free and is mature.
The Python toolchain aligns with the FreeCAD community. Switching to VitePress
or Docusaurus would add Node.js complexity without a meaningful benefit at this
project's scale (at most ~3 tracked FreeCAD versions).

The one thing to do as a result of this evaluation: **update all project
documentation to explicitly state we target the OSS release only**, so that no
future contributor (or AI agent) accidentally reaches for an Insiders feature.

---

## 7. Versioning implementation plan (using mike)

When the time comes to add versioning, the process is:

### First-time setup

```bash
pip install mike

# Add to mkdocs.yml:
# extra:
#   version:
#     provider: mike
#     default: stable

# Deploy the first version
mike deploy --push --update-aliases 1.0 stable
mike set-default --push stable
```

### GitHub Actions update (add version on each tagged release)

```yaml
- name: Deploy docs version
  if: startsWith(github.ref, 'refs/tags/freecad-')
  run: |
    git fetch origin gh-pages --depth=1
    mike deploy --push --update-aliases ${{ github.ref_name }} latest
```

### Version aliases to maintain

| Alias | Points to |
|-------|-----------|
| `stable` | Latest stable FreeCAD version (e.g. 1.0) |
| `dev` | Docs for the in-development next version |
| `1.0`, `0.21`, … | Permanent version snapshots |

The version selector in the Material theme will automatically render these aliases
as a dropdown.

---

## 8. Decision log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-05-26 | Evaluating alternatives | Concern raised about Insiders paywall and versioning |
| — | **Pending: confirm Material for MkDocs OSS** | See §6 |
