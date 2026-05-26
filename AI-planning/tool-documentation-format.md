# Proposal: Standard Tool Documentation Format

**Status:** Draft  
**Scope:** All individual tool pages across all workbenches (Sketcher, Part Design, Part, and future workbenches)

---

## 1. Problem statement

FreeCAD has hundreds of tools spread across many workbenches. Without a consistent
structure, documentation pages become hard to scan, hard to compare across tools,
and easy to write inconsistently. New contributors (human or AI) need to know
exactly what to write and in what order — without having to re-invent the
structure for every page.

This document proposes a single canonical template that every tool page must
follow.

---

## 2. Design principles

### 2.1 Progressive complexity

The single most important structural rule: **the page must get more technical as
you scroll down.** A complete beginner who stops reading halfway should still have
learned something useful. An expert who skips to the bottom should find the depth
they need without wading through introductory prose.

This maps directly to reader intent:

| Reader | Stops reading at | What they need |
|--------|-----------------|----------------|
| Total beginner | Overview / Intuition | "What even is this?" |
| Casual user | When to use / When not to use | "Is this the right tool?" |
| Active user | Walkthrough | "How do I actually do it?" |
| Power user | Parameters / Advanced | "What does this option do?" |
| Scripter | Python API | "Give me the call signature" |

No section should require the reader to have read sections that come after it.

### 2.2 Intuition before mechanics

Every tool section starts with a real-world analogy or a one-sentence mental
model before describing what buttons to press. Understanding *why* a tool works
the way it does makes every detail easier to retain.

### 2.3 Concrete over abstract

- Prefer "Select two lines, then apply this constraint — they will be forced to
  meet at a point" over "this constraint enforces geometric coincidence."
- Every parameter must have at least one example of a value that would or
  wouldn't work.
- Warnings and gotchas belong in `!!! warning` admonitions, not buried in prose.

### 2.4 Completeness at the reference level

The parameters table and Python API section must be exhaustive — they are the
reference a user reaches for when they already understand the concept and just
need a fact. All possible values, default values, and constraints must be listed.

---

## 3. The template

The following is the exact section structure every tool page must follow.
Annotations (in *italics*) explain the intent of each section; strip them when
writing real pages.

---

```markdown
# <Tool Name>

<!-- One sentence. Start with a verb. No jargon. -->
> **In one sentence:** <Pad extrudes a closed 2-D sketch into a 3-D solid by
> pushing it along its normal direction.>

---

## Overview

<!-- 2–4 sentences. Name the workbench, the menu path, and the toolbar location.
     Describe what the tool produces — not how it works yet, just what comes out.
     End with the keyboard shortcut if there is one.
     
     Format:
     - Workbench badge (use Material admonition or a simple bold label)
     - What it does in plain English
     - Where to find it (menu + toolbar)
     - Shortcut if any
-->

**Workbench:** Sketcher · **Menu:** Sketch → Sketcher geometries → <tool>  
**Toolbar:** <Toolbar name> · **Shortcut:** `<key>`

<Plain-English description of what the tool produces or achieves, 2–4 sentences.>


## Intuition

<!-- This is the most important section for new users.
     Write a concrete analogy or mental model. Avoid FreeCAD-specific vocabulary.
     The reader should finish this section able to predict roughly what the tool
     will do before they ever click it.
     
     Good test: could you explain this section to someone who has never opened
     FreeCAD?
     
     Include a single illustrative image or diagram if one exists.
-->

<Analogy or mental model, 1–3 paragraphs.>


## When to use it

<!-- A bulleted list of concrete situations that call for this tool.
     Start each bullet with a scenario, not a feature name.
     Aim for 3–6 bullets.
     
     Example:
     - You have drawn the outline of a bracket and want it to have physical
       thickness.
     - You are building a model parametrically and want the height of a boss
       to be driven by a named dimension.
-->

- <Scenario 1>
- <Scenario 2>
- <…>


## When NOT to use it

<!-- Just as important as the previous section.
     List the common mistakes — cases where a beginner would reach for this tool
     but should use something else instead.
     Name the alternative tool explicitly.
     Aim for 2–5 bullets.
-->

- **Don't use this when <situation>** — use [<Alternative Tool>](<link>) instead.
- <…>


## Step-by-step walkthrough

<!-- The first time a user actually learns to use the tool.
     Write it as a numbered procedural list.
     Each step is one action. No step should contain more than one click or
     decision.
     
     Where a step has multiple ways to do it (menu vs toolbar vs shortcut), note
     all of them.
     Where a step can go wrong, add a nested !!! warning.
     
     End with what the user should see when they have finished.
-->

1. <Prerequisite: what must exist before you start.>
2. <Select / activate / open …>
3. <Fill in / click / set …>
4. <Confirm with OK or press Enter.>
5. <What the result looks like in the viewport.>

!!! tip
    <Optional: one workflow shortcut or efficiency note.>


## Parameters

<!-- The reference table. Must be exhaustive — every option that appears in the
     task panel or dialog, in the order it appears in the UI.
     
     Columns:
     - Parameter: exact label as shown in the UI
     - Type: the kind of input (length, angle, integer, toggle, dropdown, …)
     - Default: the factory default value
     - Description: what it does; include valid range or allowed values
-->

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| <Label> | <Type> | <Default> | <What it controls. Valid range or options if applicable.> |
| … | … | … | … |


## Advanced usage

<!-- Optional section — include only if there are non-obvious techniques that
     experienced users would want.
     
     Topics that typically belong here:
     - Combining this tool with others in a non-obvious way
     - Symmetry / attachment mode interactions
     - Performance considerations for large models
     - How the tool behaves with multi-body setups
-->

<Content, or omit section entirely if there is nothing genuinely advanced to say.>


## Common mistakes and pitfalls

<!-- A short troubleshooting reference. Use !!! warning admonitions.
     Each block: symptom → cause → fix.
     Aim for the top 3–5 issues that real users hit.
-->

!!! warning "<Symptom: e.g. Sketch is not closed>"
    **Cause:** <Why this happens.>  
    **Fix:** <What to do about it.>

!!! warning "<Symptom>"
    **Cause:** <…>  
    **Fix:** <…>


## Python API

<!-- The most technical section. Comes last.
     
     Show the minimal scripting example first (copy-pasteable, runnable).
     Then give the full method signature with all parameters documented.
     Then list any related properties on the resulting feature object.
     
     All code must be verified against ../FreeCAD/src/Mod/*/App/*.
     Mark anything unverified with a # TODO: verify comment.
-->

### Minimal example

\`\`\`python
import FreeCAD as App
import <Workbench>

doc = App.activeDocument()
# <Minimal example that produces the feature>
\`\`\`

### Method signature

\`\`\`python
<module>.<MethodName>(
    <param1>,   # <type> — <description>
    <param2>,   # <type> — <description>
)
\`\`\`

### Resulting feature properties

| Property | Type | Description |
|----------|------|-------------|
| `<PropertyName>` | `<App::PropertyType>` | <What it stores.> |
| … | … | … |


## See also

<!-- Cross-links to related tools. Keep it short — 3–8 links.
     Prefer links to tools that are commonly used with this one, or that are
     commonly confused with it.
-->

- [<Related Tool 1>](<relative-link>)
- [<Related Tool 2>](<relative-link>)
```

---

## 4. Section-by-section rationale

| Section | Required? | Why |
|---------|-----------|-----|
| One-sentence summary | Yes | First hit in search results; must stand alone |
| Overview | Yes | Locates the tool for the user (workbench, menu, shortcut) |
| Intuition | Yes | Retention; prevents "I understand the UI but not the concept" |
| When to use | Yes | Decision support; user arrived here to decide, not just to learn |
| When NOT to use | Yes | Saves the user from going down the wrong path |
| Step-by-step walkthrough | Yes | Task-oriented learning; drives first use |
| Parameters | Yes | Reference; must exist even if all defaults are fine |
| Advanced usage | No | Only when there is genuinely non-obvious depth |
| Common mistakes | Yes | Reduces support burden; surfaces hard-won knowledge |
| Python API | Yes | Every tool must be scriptable; scripters deserve docs too |
| See also | Yes | Navigation; prevents dead ends |

---

## 5. Length guidance

| Section | Target length |
|---------|--------------|
| One-sentence summary | 1 sentence, max 20 words |
| Overview | 3–5 lines |
| Intuition | 100–300 words |
| When to use | 3–6 bullets |
| When NOT to use | 2–5 bullets |
| Walkthrough | 4–10 numbered steps |
| Parameters | One row per UI control |
| Advanced usage | 0–400 words |
| Common mistakes | 2–5 `!!! warning` blocks |
| Python API | 1 runnable example + signature table |
| See also | 3–8 links |

Total target: **600–1 200 words** of prose per page (not counting tables and
code blocks). Pages significantly outside this range should be reviewed — too
short likely means missing depth, too long likely means scope creep.

---

## 6. Naming and file location

```
docs/
└── <workbench>/
    └── tools/
        └── <tool-slug>.md
```

- **Workbench slugs:** `sketcher`, `part-design`, `part`
- **Tool slugs:** lowercase, hyphen-separated, match the tool's UI name as
  closely as possible. Drop "Sketcher " / "Part Design " prefixes.
  - `Sketcher Pad` → `part-design/tools/pad.md`
  - `Sketcher Coincident Constraint` → `sketcher/tools/constraint-coincident.md`
  - `Part Boolean Cut` → `part/tools/boolean-cut.md`

The page title (`# <Tool Name>`) uses the full UI name exactly as it appears in
FreeCAD's menu, including capitalisation.

---

## 7. Review checklist

Before merging any tool page, verify:

- [ ] All sections are present (or explicitly omitted with justification for
      Advanced usage)
- [ ] The one-sentence summary starts with a verb and contains no jargon
- [ ] The Intuition section contains no step-by-step instructions (those belong
      in the walkthrough)
- [ ] Every parameter in the FreeCAD task panel appears in the Parameters table
- [ ] At least one `!!! warning` block exists in Common mistakes
- [ ] The Python example has been tested or is marked `# TODO: verify`
- [ ] All internal links resolve
- [ ] Progressive complexity is intact — the page gets harder as you scroll

---

## 8. Open questions

- **Screenshots:** Should every page require at least one screenshot? Proposal:
  yes for the walkthrough, optional elsewhere. Screenshots age badly across
  FreeCAD versions and need a clear versioning policy.
- **Video embeds:** Material for MkDocs supports embedding via custom HTML.
  Worth investigating for complex tools (Loft, Swept Pad, …).
- **Insiders features:** ~~Decide whether to target OSS or Insiders.~~
  **Resolved — OSS only.** Do not use any Insiders-gated feature. All
  admonition types, content tabs, code blocks, mermaid diagrams, and the
  full PyMdown extension suite are available in OSS and are sufficient for
  every template section. See `AI-planning/site-generator-decision.md` for
  the full analysis.
- **Versioning:** FreeCAD 1.0 is the baseline. How do we handle 0.21 →
  1.0 behaviour differences? Suggest a `!!! info "Changed in 1.0"` admonition
  convention, to be formalised in a separate proposal.
