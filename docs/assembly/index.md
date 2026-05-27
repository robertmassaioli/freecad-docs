# Assembly Workbench

The Assembly workbench (introduced in FreeCAD 1.0) provides a constraint-based
system for positioning multiple parts relative to each other and simulating
their motion. It is FreeCAD's built-in assembly tool, replacing the need for
third-party add-ons for most common assembly tasks.

---

## What the Assembly workbench is for

Assembly answers the question: *how do these parts fit and move together?*

Given a set of individual parts — a shaft, a housing, bearings, bolts — Assembly
lets you:

1. **Insert** the parts into an assembly container.
2. **Ground** one part to fix it in space as the reference.
3. **Add joints** (constraints) that define the allowed relative motion between
   pairs of parts: a revolute joint makes a shaft spin inside its housing; a
   slider joint makes a piston move along a cylinder bore.
4. **Solve** the assembly — FreeCAD moves each part to satisfy all joints
   simultaneously.
5. **Animate** the result — use the Simulation tool to drive a joint through a
   range of motion and watch the mechanism move.
6. **Explode** the assembly for documentation and presentations.
7. **Generate a Bill of Materials** listing every component.

---

## Key concepts

### Assembly object and activation

An assembly is an `Assembly::AssemblyObject` — a container that holds parts and
joints. You can have multiple assemblies in one document (sub-assemblies). Only
one assembly is *active* at a time; joints and inserts apply to the active one.

### Links, not copies

Parts are inserted as **links** (App::Link) rather than copies. A link
references the original part geometry; no second copy of the geometry is stored.
Changing the part (e.g. increasing a pad length) automatically updates the
assembly. The link carries its own placement (position + orientation) inside the
assembly.

### Joints

A **joint** is a kinematic constraint between two parts. Each joint defines how
many degrees of freedom (DOF) the two parts have relative to each other:

- **0 DOF** (Fixed joint) — no relative motion; the parts are locked together.
- **1 DOF** — one rotation (Revolute) or one translation (Slider).
- **2 DOF** — rotation + translation along the same axis (Cylindrical).
- **3 DOF** — free rotation about a point (Ball).

Adding joints reduces DOF until the assembly is fully constrained (each free
part starts with 6 DOF: 3 translations + 3 rotations).

### Grounding

Every assembly needs at least one **grounded** part — a part whose position is
fixed in the assembly frame. Without a ground, the solver has no reference and
all parts float. Typically the base plate, chassis, or frame of a mechanism is
grounded.

### The solver

The Assembly workbench uses an integrated constraint solver. After adding joints,
press **Solve Assembly** to compute positions. The solver reports success (all
joints satisfied) or failure (over-constrained, under-constrained, or
conflicting joints).

---

## Typical workflow

```
1. Create Assembly          ← Assembly → New Assembly
2. Insert parts             ← Assembly → Insert Component (repeat)
3. Ground one part          ← Assembly → Toggle Grounded
4. Add joints               ← Assembly → Joints menu (repeat)
5. Solve                    ← Assembly → Solve Assembly
6. (Optional) Animate       ← Assembly → Simulation
7. (Optional) Explode       ← Assembly → Exploded View
8. (Optional) BOM           ← Assembly → Bill of Materials
```

---

## Relationship to other workbenches

| Workbench | Relationship |
|-----------|-------------|
| **Part Design** | Bodies created in Part Design are inserted as components. |
| **Part** | Part shapes can be inserted as components. |
| **Sketcher** | Sketches referenced from parts update assembly positions when the sketch changes. |
| **Spreadsheet** | Part dimensions driven by a spreadsheet update the assembly automatically on recompute. |
| **TechDraw** | Assembly views and exploded views can be projected into TechDraw drawings. |

See the [Tools index](tools/index.md) for a complete reference.
