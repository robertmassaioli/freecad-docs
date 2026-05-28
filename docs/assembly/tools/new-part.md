# New Part

> **In one sentence:** Create a new empty Part Design body inside the active assembly so you can model the part in-context, with all surrounding assembly parts visible as references.

---

## Overview

**Workbench:** Assembly  
**Menu:** Assembly → Insert Component → New Part  
**Shortcut:** none

New Part creates a new `PartDesign::Body` inside the assembly's `Parts` container and activates it for editing. You can then use Part Design tools normally while seeing all other assembly parts in the 3-D view — allowing geometry to be modelled with reference to adjacent parts.

---

## Intuition

Sometimes a part's geometry is fundamentally defined by the assembly context around it. A custom bracket that must span between two specific faces, a spacer whose length equals the gap between two components, a cover plate whose outline exactly follows the assembly footprint — these parts can only be designed meaningfully when you can see the assembly.

New Part gives you exactly this: a blank Body inside the assembly, ready to model, with every other inserted part visible and available as external geometry references in your sketches.

---

## When to use it

- The part's geometry depends on the exact positions or shapes of neighbouring parts.
- You are designing a custom interface component (bracket, adapter, spacer) that fits a specific assembly configuration.
- You want to model a part and immediately see how it fits without switching documents.

## When NOT to use it

- **Don't use this for standalone, reusable parts** — model those independently in their own document and use [Insert Component](insert-component.md). Parts created with New Part live inside the assembly document and cannot easily be referenced by other assemblies.
- **Don't use this to import existing parts** — use [Insert Component](insert-component.md) for parts that already exist.

---

## Step-by-step

1. Make sure the assembly is active.
2. Choose **Assembly → Insert Component → New Part**.
3. A new `Body` appears in the model tree under `Parts` and is immediately activated (shown in bold).
4. Switch to the **Sketcher** or **Part Design** workbench.
5. Model the part normally — sketches, pads, pockets, etc.
6. Other assembly parts are visible in the 3-D view. Use **Sketcher → External Geometry** to reference their edges and faces.
7. When finished, double-click the **Assembly** object in the model tree to re-activate the assembly.
8. Add joints to position the new part relative to the others.

!!! tip
    If you need the new part to sit flush against another part's face, place
    your first sketch directly on that face — the sketch will be attached to
    the face and update if the face moves.

---

## Parameters

The Body created by New Part has the same parameters as any Part Design Body — no special assembly-specific parameters.

---

## Common mistakes and pitfalls

!!! warning "Forgetting to re-activate the assembly"
    After modelling the new part, the Body remains active. If you then try to
    Insert Component or add joints, they will be applied to the Body rather
    than the assembly. Always double-click the Assembly object to return to
    assembly context.

!!! warning "In-context parts are not reusable"
    The Body created by New Part lives inside the assembly document. To use
    it in another assembly, you must save it as a separate file. Consider
    whether the part should be standalone before choosing this approach.

!!! warning "External geometry references can break"
    If you reference a face of another assembly part via External Geometry
    and then change that part significantly (removing or reordering features),
    the reference may break. Check External Geometry links when updating
    referenced parts.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument("Assembly")

# Create assembly
asm = doc.addObject("Assembly::AssemblyObject", "Assembly")
doc.recompute()

# Create a new body inside the assembly
body = doc.addObject("PartDesign::Body", "NewPart")
asm.getObject("Parts").addObject(body)
doc.recompute()

# The body is now inside the assembly and ready for Part Design features
```

---

## See also

- [Insert Component](insert-component.md) — add an existing part instead
- [New Assembly](new-assembly.md) — create the assembly container first
- [Toggle Grounded](grounded.md) — ground a part before adding joints
