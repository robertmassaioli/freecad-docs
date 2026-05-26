# Part Design Tools

Complete reference index for all Part Design workbench tools, grouped by
function. Each tool name links to its dedicated page.

---

## Structure & setup { #structure }

Tools for creating the model container, attaching sketches, and defining
reference geometry that other features snap to.

| Tool | Description |
|------|-------------|
| [Body](body.md) | Create a new Body — the mandatory container for all Part Design features |
| [New Sketch](new-sketch.md) | Create a sketch attached to a face or datum plane within the active Body |
| [Datum Plane](datum-plane.md) | Insert a parametric reference plane at any position or angle |
| [Datum Line](datum-line.md) | Insert a parametric reference line (axis) |
| [Datum Point](datum-point.md) | Insert a parametric reference point |
| [Coordinate System](coordinate-system.md) | Insert a local coordinate system (all three datum axes at once) |
| [Shape Binder](shape-binder.md) | Copy external geometry into the Body as a non-solid reference |
| [Sub-Shape Binder](sub-shape-binder.md) | Reference a specific face, edge, or vertex from another Body |
| [Clone](clone.md) | Insert a linked clone of another Body |

---

## Additive features { #additive }

Sketch-based tools that **add** material to the Body.

| Tool | Description |
|------|-------------|
| [Pad](pad.md) | Extrude a closed sketch perpendicular to its plane to create a solid |
| [Revolution](revolution.md) | Revolve a sketch profile around an axis to create a solid of revolution |
| [Additive Loft](additive-loft.md) | Blend through two or more sketch profiles to create a smooth solid |
| [Additive Pipe](additive-pipe.md) | Sweep a sketch profile along a path curve |
| [Additive Helix](additive-helix.md) | Sweep a sketch profile along a helical path |

---

## Additive primitives { #primitives-additive }

Insert parametric solid shapes without a sketch, adding material to the Body.

| Tool | Description |
|------|-------------|
| [Additive Box](additive-box.md) | Add a rectangular box |
| [Additive Cylinder](additive-cylinder.md) | Add a cylinder |
| [Additive Sphere](additive-sphere.md) | Add a sphere |
| [Additive Cone](additive-cone.md) | Add a cone or truncated cone |
| [Additive Ellipsoid](additive-ellipsoid.md) | Add an ellipsoid |
| [Additive Torus](additive-torus.md) | Add a torus (donut shape) |
| [Additive Prism](additive-prism.md) | Add a regular polygonal prism |
| [Additive Wedge](additive-wedge.md) | Add a wedge (parametric tapered box) |

---

## Subtractive features { #subtractive }

Sketch-based tools that **remove** material from the Body.

| Tool | Description |
|------|-------------|
| [Pocket](pocket.md) | Extrude a cut into the solid — the inverse of Pad |
| [Groove](groove.md) | Revolve a cut around an axis — the inverse of Revolution |
| [Hole](hole.md) | Create an engineered hole with optional thread, counterbore, or countersink |
| [Subtractive Loft](subtractive-loft.md) | Blend-cut through two or more sketch profiles |
| [Subtractive Pipe](subtractive-pipe.md) | Sweep a cut along a path curve |
| [Subtractive Helix](subtractive-helix.md) | Sweep a cut along a helical path |

---

## Subtractive primitives { #primitives-subtractive }

Insert parametric solid shapes that **remove** material from the Body.

| Tool | Description |
|------|-------------|
| [Subtractive Box](subtractive-box.md) | Remove a rectangular box |
| [Subtractive Cylinder](subtractive-cylinder.md) | Remove a cylinder |
| [Subtractive Sphere](subtractive-sphere.md) | Remove a sphere |
| [Subtractive Cone](subtractive-cone.md) | Remove a cone or truncated cone |
| [Subtractive Ellipsoid](subtractive-ellipsoid.md) | Remove an ellipsoid |
| [Subtractive Torus](subtractive-torus.md) | Remove a torus |
| [Subtractive Prism](subtractive-prism.md) | Remove a regular polygonal prism |
| [Subtractive Wedge](subtractive-wedge.md) | Remove a wedge |

---

## Dress-up features { #dressup }

Apply finishing operations to edges and faces of an existing solid.

| Tool | Description |
|------|-------------|
| [Fillet](fillet.md) | Round selected edges with a constant or variable radius |
| [Chamfer](chamfer.md) | Bevel selected edges at a specified angle and distance |
| [Draft](draft.md) | Taper selected faces by a draft angle — useful for mould tooling |
| [Thickness](thickness.md) | Hollow a solid to a uniform wall thickness by removing selected faces |

---

## Transformation features { #transformations }

Repeat or mirror existing features without redrawing them.

| Tool | Description |
|------|-------------|
| [Mirrored](mirrored.md) | Mirror one or more features across a plane |
| [Linear Pattern](linear-pattern.md) | Repeat features in a straight line with a fixed spacing |
| [Polar Pattern](polar-pattern.md) | Repeat features in a circular array around an axis |
| [Multi-Transform](multi-transform.md) | Combine multiple transformation types into a single compound pattern |

---

## Boolean operations { #boolean }

Combine multiple Bodies using Boolean set operations.

| Tool | Description |
|------|-------------|
| [Boolean](boolean.md) | Union, subtract, or intersect two Bodies |

---

## Specialised tools { #specialised }

| Tool | Description |
|------|-------------|
| [Involute Gear](involute-gear.md) | Generate a parametric involute gear profile sketch |
| [Sprocket](sprocket.md) | Generate a parametric sprocket profile sketch |
| [Shaft Wizard](wizard-shaft.md) | Interactive wizard for designing stepped shafts |

---

## Tree management { #tree }

UI operations for reorganising the feature tree. These are workflow tools, not
modelling features; they do not change the final shape, only the tree structure.

| Tool | Description |
|------|-------------|
| [Move Tip](move-tip.md) | Set the active Tip to any feature in the tree |
| [Move Feature](move-feature.md) | Move a feature to a different Body |
| [Move Feature in Tree](move-feature-in-tree.md) | Reorder a feature within the same Body's tree |
| [Duplicate Selection](duplicate-selection.md) | Duplicate the selected feature(s) within the Body |
