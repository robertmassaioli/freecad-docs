# Part Design Workbench

The **Part Design** workbench is FreeCAD's primary tool for creating parametric
solid models using a **feature-based workflow**. You build a model by applying an
ordered sequence of operations to a single container called a *Body*. Each
operation is recorded in the model tree so that changing an early dimension
automatically updates everything that depends on it.

!!! info "FreeCAD version"
    This documentation covers **FreeCAD 1.1** (release 1.1.1).

## Key concepts

**Body**
:   The single solid container that holds all features. Every Part Design model
    starts by creating a Body. A document can contain multiple Bodies, but each
    Body produces one independent solid.

**Feature tree**
:   The ordered history of operations inside the Body (Pad, Pocket, Fillet, …).
    FreeCAD applies them top-to-bottom; the resulting shape is the cumulative
    effect of all features in order.

**Tip**
:   The currently active feature — the last one in the tree by default. The Tip
    determines which shape is visible in the viewport and which operations have
    been "applied" to the model so far.

**Sketch-based vs. primitive features**
:   Most additive and subtractive features are *sketch-based*: you draw a 2-D
    profile in the Sketcher workbench, then tell Part Design what to do with it
    (extrude it, revolve it, sweep it, …). *Primitive* features (Box, Sphere, …)
    skip the sketch and insert a parametric geometric shape directly.

## Tool reference

See the **[Tools index](tools/index.md)** for a complete, grouped list of every
tool with one-line descriptions.

## Tool groups

| Group | Purpose |
|-------|---------|
| [Structure & setup](tools/index.md#structure) | Body, datum geometry, sketch attachment, binders |
| [Additive features](tools/index.md#additive) | Add material via sketches: Pad, Revolution, Loft, Pipe, Helix |
| [Additive primitives](tools/index.md#primitives-additive) | Add material via solid shapes: Box, Cylinder, Sphere, … |
| [Subtractive features](tools/index.md#subtractive) | Remove material via sketches: Pocket, Groove, Hole, … |
| [Subtractive primitives](tools/index.md#primitives-subtractive) | Remove material via solid shapes |
| [Dress-up features](tools/index.md#dressup) | Refine edges and faces: Fillet, Chamfer, Draft, Thickness |
| [Transformation features](tools/index.md#transformations) | Pattern and mirror features |
| [Boolean](tools/index.md#boolean) | Combine or subtract entire Bodies |
| [Specialised](tools/index.md#specialised) | Gear, sprocket, shaft wizard |
| [Tree management](tools/index.md#tree) | Reorder, move, and duplicate features |

## Typical workflow

```
1. Create a Body                     (Part Design → Body)
2. Create a Sketch on a face/plane   (Part Design → New Sketch)
3. Draw and constrain the profile    (Sketcher workbench)
4. Close the sketch
5. Apply an additive feature         (e.g. Pad)
6. Repeat: sketch → feature          (building up the model)
7. Apply dress-up features           (Fillet, Chamfer, …)
8. Apply transformations if needed   (Pattern, Mirror)
```
