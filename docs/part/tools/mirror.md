# Mirror

> **In one sentence:** Create a mirrored copy of a shape reflected about one of
> the three standard planes (XY, XZ, YZ) or a custom plane.

---

## Overview

**Workbench:** Part · **Menu:** Part → Mirror  
**Toolbar:** Part Modeling · **Shortcut:** none

Mirror creates a new `Part::Mirror` object containing the reflection of the
selected shape about a specified plane. The original shape is preserved; the
mirror is parametric and updates when the original changes.

---

## Intuition

Most mechanical parts have at least one plane of symmetry. Rather than
modelling both halves, model one half and mirror it. The mirrored copy updates
automatically when the original changes, keeping both halves in sync.

Part Mirror differs from Part Design Mirrored: Part Mirror creates a standalone
mirror copy; Part Design Mirrored is a feature inside a Body that applies to the
tip solid.

---

## When to use it

- Creating symmetric parts from a single half-model.
- Quickly testing what the mirrored version of a design looks like.
- Generating symmetric compound assemblies.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Shape | Part object | — | The shape to mirror |
| Mirror Plane | XY / XZ / YZ | XZ | The reflection plane (passes through origin) |
| Base X, Y, Z | Distance | 0, 0, 0 | Position of the plane origin |

---

## Step-by-step walkthrough

1. **Select the shape** to mirror.
2. **Part → Mirror.**
3. In the dialog, choose the **Mirror Plane** (XY, XZ, or YZ).
4. Adjust **Base X/Y/Z** if the mirror plane should not pass through the world
   origin.
5. Click **OK**.

---

## Python API

```python
import FreeCAD as App

doc = App.newDocument()

original = doc.addObject("Part::Box", "HalfBlock")
original.Length = 20; original.Width = 10; original.Height = 5
doc.recompute()

# Mirror about XZ plane (reflect in Y)
mirror = doc.addObject("Part::Mirror", "Mirrored")
mirror.Source = original
mirror.MirrorPlane = "XZ"  # XY, XZ, or YZ
# Position the plane at Y=0 (origin) — default
doc.recompute()

# Fuse with original to get full symmetric part
fuse = doc.addObject("Part::Fuse", "Full")
fuse.Base = original
fuse.Tool = mirror
doc.recompute()
print(f"Full part volume: {fuse.Shape.Volume:.2f} mm³")
```

---

## Common mistakes and pitfalls

!!! warning "Mirror plane passes through world origin by default"
    If your shape is not symmetric about the origin, the mirrored copy will be
    at the wrong position. Set **Base** to the position of your symmetry plane.

!!! warning "Mirror creates a copy — it does not flip the original"
    The original shape is unchanged. If you want to flip the shape (not copy
    it), use Placement rotation instead.

---

## See also

- [Extrude](extrude.md) — linear extrusion
- [Scale](scale.md) — non-uniform scaling (can also flip by using negative scale)
- [Compound Tools](compound.md) — group mirror + original into a compound
