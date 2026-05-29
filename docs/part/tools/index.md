# Part Tools

Complete index of all Part workbench tools, grouped by function.

---

## Primitives

Parametric shapes that can be resized at any time by changing their properties.

| Tool | Description |
|------|-------------|
| [Cube](cube.md) | Parametric rectangular box |
| [Cylinder](cylinder.md) | Parametric cylinder |
| [Sphere](sphere.md) | Parametric sphere |
| [Cone](cone.md) | Parametric cone or frustum |
| [Torus](torus.md) | Parametric torus (donut) |
| [Tube](tube.md) | Parametric hollow cylinder with inner and outer radius |
| [Primitives dialog](primitives.md) | Additional parametric shapes: Plane, Helix, Spiral, Circle, Ellipse, Wedge, Vertex, Line, Regular Polygon |
| [Shape Builder](shape-builder.md) | Compose new shapes from vertices, edges, wires, and faces |

---

## Mesh and Solid Conversion

| Tool | Description |
|------|-------------|
| [Shape from Mesh](shape-from-mesh.md) | Wrap a mesh object as a Part shape |
| [Points from Shape](points-from-shape.md) | Extract vertices from a shape as a Points object |
| [Convert to Solid](convert-to-solid.md) | Convert a shell or surface shape to a closed solid |
| [Reverse Shapes](reverse-shapes.md) | Flip the face normals of a shape |

---

## Copy Tools

| Tool | Description |
|------|-------------|
| [Simple Copy](simple-copy.md) | Create a static, non-parametric copy of a shape |
| [Transformed Copy](transformed-copy.md) | Copy a shape with an applied placement transform |
| [Shape Element Copy](shape-element-copy.md) | Copy a single face, edge, or vertex from a shape |
| [Refine Shape](refine-shape.md) | Remove redundant edges and merge co-planar faces |

---

## Boolean Operations

| Tool | Description |
|------|-------------|
| [Boolean (dialog)](boolean.md) | General Boolean dialog supporting Cut, Fuse, Common, and Section on two selected shapes |
| [Cut](boolean-cut.md) | Subtract one shape from another |
| [Fuse (Union)](boolean-fuse.md) | Merge two shapes into one |
| [Common (Intersection)](boolean-common.md) | Keep only the volume shared by two shapes |

---

## Join Features

Topology-aware operations for walled/hollow solids that preserve their hollow
character through Boolean-like joins.

| Tool | Description |
|------|-------------|
| [Connect Shapes](connect-shapes.md) | Fuse two walled objects sharing an edge, connecting their interiors |
| [Embed Shapes](embed-shapes.md) | Embed a smaller walled solid inside a larger one |
| [Cutout Shape](cutout-shape.md) | Cut a hole in a walled solid matching an embedded object |

---

## Split Features

Advanced Boolean operations that split shapes rather than just merging or removing
them.

| Tool | Description |
|------|-------------|
| [Boolean Fragments](boolean-fragments.md) | Split all input shapes at every mutual intersection |
| [Slice Apart](slice-apart.md) | Cut a shape into separate objects using a tool shape |
| [Slice to Compound](slice-to-compound.md) | Cut a shape and keep all fragments as a single compound |
| [Boolean XOR](boolean-xor.md) | Keep only the non-overlapping portions of two shapes |

---

## Compound Tools

| Tool | Description |
|------|-------------|
| [Make Compound](compound.md#make-compound) | Group multiple shapes into a single compound object |
| [Explode Compound](compound.md#explode-compound) | Split a compound back into its constituent shapes |
| [Compound Filter](compound.md#compound-filter) | Extract specific sub-shapes from a compound by type or index |

---

## Modeling Tools

| Tool | Description |
|------|-------------|
| [Extrude](extrude.md) | Push a face or wire along a direction to create a solid or shell |
| [Revolve](revolve.md) | Spin a profile around an axis to create a solid of revolution |
| [Mirror](mirror.md) | Reflect a shape about a plane |
| [Scale](scale.md) | Scale a shape by XYZ factors |
| [Fillet](fillet.md) | Round selected edges of a shape |
| [Chamfer](chamfer.md) | Bevel selected edges of a shape |
| [Face from Wires](face-from-wires.md) | Create a flat face from one or more closed wire profiles |
| [Ruled Surface](ruled-surface.md) | Create a surface by linearly blending two edges or wires |
| [Loft](loft.md) | Blend through a series of cross-section profiles to form a solid |
| [Sweep](sweep.md) | Sweep a profile along a spine path to create a solid |
| [Section](section.md) | Find the cross-section wire where two shapes intersect |
| [Cross-Sections](cross-sections.md) | Generate multiple parallel cross-section wires through a shape |
| [Offset 3D Surface](offset-3d.md) | Offset all faces of a solid outward or inward |
| [Offset 2D](offset-2d.md) | Offset a 2-D wire or face in its plane |
| [Thickness](thickness.md) | Hollow out a solid to a uniform wall thickness |
| [Projection on Surface](projection-on-surface.md) | Project wires, edges, or faces onto a target surface |

---

## Analysis Tools

| Tool | Description |
|------|-------------|
| [Check Geometry](check-geometry.md) | Analyse a shape for OCCT validity errors |
| [Defeaturing](defeaturing.md) | Remove selected faces from a solid to simplify it |
| [Persistent Section Cut](section-cut.md) | Live clipping plane that cuts a persistent cross-section view |
| [Appearance per Face](appearance-per-face.md) | Assign different materials or colours to individual faces |
