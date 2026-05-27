# Thickness

> **In one sentence:** Hollow out a solid to a uniform wall thickness by
> removing selected faces and offsetting the remaining shell inward.

---

## Overview

**Workbench:** Part · **Menu:** Part → Thickness  
**Toolbar:** Part Modeling · **Shortcut:** none

Thickness takes a solid, removes one or more selected faces (the "openings"),
and offsets the remaining faces inward by the specified thickness, producing a
hollow shell. It is the standard operation for creating injection-moulded plastic
parts or sheet-metal shells.

---

## Intuition

Imagine pressing a thick block of clay into a mould with a flat bottom. The
result is a hollow box: the clay forms the walls and bottom, with the top open.
Part Thickness does the same digitally: you pick which faces become openings,
and FreeCAD creates the hollow shell at your specified wall thickness.

---

## When to use it

- Creating injection-moulded plastic housing from a solid model.
- Producing a hollow container (box, tube, vessel) from a solid primitive.
- Sheet-metal shell modelling (followed by flattening in a sheet-metal workbench).

## When NOT to use it

- **Hollow cylinder** — use [Tube](tube.md) which is parametric.
- **Uniform 3-D offset** — use [Offset 3D](offset-3d.md) if no faces are removed.

---

## Step-by-step walkthrough

1. **Select the solid** in the model tree.
2. In the viewport, **Ctrl+click the face(s)** that will become the opening.
3. **Part → Thickness.**
4. In the dialog, set the **Thickness** value.
5. Set **Mode** (Skin) and **Join** type (Arc, Intersection, or Tangent).
6. Click **OK**.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Faces | Face list | — | Faces to remove (openings) |
| Value | Distance | 1 mm | Wall thickness |
| Mode | Skin / Pipe / RectoVerso | Skin | How the offset is constructed |
| Join | Arc / Intersection / Tangent | Arc | How corners are treated |
| Make Solid | Bool | Yes | Create a closed solid (not a shell) |
| Reversed | Bool | No | Offset outward instead of inward |
| Intersection | Bool | No | Handle self-intersections |

---

## Python API

```python
import FreeCAD as App
import Part

doc = App.newDocument()

box = doc.addObject("Part::Box", "Box")
box.Length = 30; box.Width = 20; box.Height = 20
doc.recompute()

# Hollow the box with 2 mm walls, top face open
# Top face of the box is Faces[5] (index 5 for the default box)
opening_face = box.Shape.Faces[5]

thick = doc.addObject("Part::Thickness", "Shell")
thick.Faces  = [(box, ["Face6"])]  # FreeCAD uses 1-based face names: Face6 = Faces[5]
thick.Value  = 2.0                  # mm wall thickness
thick.Mode   = "Skin"
thick.Join   = 0                    # 0=Arc, 1=Intersection, 2=Tangent
doc.recompute()

print(f"Shell volume: {thick.Shape.Volume:.2f} mm³")
```

> **Note:** Face selection in Thickness uses the FreeCAD face name string
> (e.g., `"Face6"`) rather than the Python list index. Use the viewport
> selection to pick faces interactively.

---

## Common mistakes and pitfalls

!!! warning "Thickness larger than smallest feature"
    If the specified thickness is larger than the smallest feature of the solid,
    OCCT produces an error or self-intersecting geometry. Ensure wall thickness
    is less than half the smallest dimension.

!!! warning "Must select at least one opening face"
    Without selecting an opening face, Thickness tries to offset all faces
    inward equally, producing an OCCT error for a closed solid (the result would
    be a solid inside a solid with zero-thickness walls). Always select the faces
    to open.

---

## See also

- [Offset 3D](offset-3d.md) — uniform 3-D offset without removing faces
- [Tube](tube.md) — hollow cylinder primitive
- [Fillet](fillet.md) — round the sharp edges of the shell result
