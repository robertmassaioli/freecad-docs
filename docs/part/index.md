# Part Workbench

The Part workbench provides direct access to OCCT (Open CASCADE Technology)
shapes and Boolean operations. Unlike Part Design — which chains features inside
a Body — Part operates on *arbitrary shapes* with no Body container required.

---

## What the Part workbench is for

Part gives you three things:

1. **Parametric primitives** — boxes, cylinders, spheres, cones, tori, and more.
   Each is a live object whose dimensions can be changed at any time.

2. **Shape operations** — extrude, revolve, loft, sweep, fillet, chamfer, offset,
   and many more operations that work on any imported or constructed shape.

3. **Boolean algebra** — cut one shape from another (Part Cut), fuse two shapes
   into one (Part Fuse), find their intersection (Part Common), or use the
   advanced Boolean Fragments / Join / Split tools for complex overlap geometry.

Part is the lower-level workbench. Part Design wraps Part operations in a more
opinionated feature-tree workflow. You would reach for Part directly when:

- You are working with imported STEP/IGES geometry that does not live inside a Body.
- You need operations that Part Design does not expose (Boolean Fragments, Loft
  across arbitrary profiles, Ruled Surface).
- You are scripting shape generation via Python.

---

## Key concepts

### Shapes vs Features

Every Part object is a OCCT `TopoDS_Shape` — a B-rep (boundary representation)
solid, shell, face, wire, edge, or vertex. Operations on shapes produce new shapes;
the original shapes remain and can be referenced again.

### Parametric primitives

Primitives (Box, Cylinder, …) created in the Part workbench are fully parametric:
open the Data panel and change any dimension, and the shape updates. They can also
be attached to faces, datums, or axes using the Attachment framework.

### Topological naming

Because Part operates on arbitrary shapes without a strict feature order, it is
susceptible to the *topological naming problem*: when you add or remove a feature,
face/edge/vertex names can change, breaking references in downstream features.
FreeCAD 1.0+ includes topological naming mitigations that reduce (but do not
eliminate) this issue.

---

## Tools overview

| Group | Tools |
|-------|-------|
| [Primitives](tools/index.md#primitives) | Cube, Cylinder, Sphere, Cone, Torus, Tube, Primitives dialog, Shape Builder |
| [Mesh & Solid](tools/index.md#mesh-and-solid-conversion) | Shape from Mesh, Points from Mesh, Convert to Solid, Reverse Shapes |
| [Copy](tools/index.md#copy-tools) | Simple Copy, Transformed Copy, Element Copy, Refine Shape |
| [Boolean](tools/index.md#boolean-operations) | Cut, Fuse, Common, Boolean dialog |
| [Join](tools/index.md#join-features) | Connect Shapes, Embed Shapes, Cutout Shape |
| [Split](tools/index.md#split-features) | Boolean Fragments, Slice Apart, Slice to Compound, Boolean XOR |
| [Compound](tools/index.md#compound-tools) | Make Compound, Explode Compound, Compound Filter |
| [Modeling](tools/index.md#modeling-tools) | Extrude, Revolve, Mirror, Scale, Fillet, Chamfer, Face from Wires, Ruled Surface, Loft, Sweep, Section, Offset 3D/2D, Thickness, Projection on Surface |
| [Analysis](tools/index.md#analysis-tools) | Check Geometry, Defeaturing, Section Cut, Appearance per Face |

---

## Relationship to other workbenches

| Workbench | Relationship |
|-----------|-------------|
| **Sketcher** | Provides 2-D profiles for Extrude, Revolve, Sweep, Loft, and Face from Wires. |
| **Part Design** | A higher-level workflow built on top of Part operations. Prefer Part Design for solid modelling within a Body; use Part for operations outside a Body or not available in Part Design. |

See the [Tools index](tools/index.md) for a complete reference.
