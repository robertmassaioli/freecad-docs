# Thickness

> **In one sentence:** Hollow out a solid by removing one or more selected faces and offsetting the remaining faces inward (or outward) by a uniform wall thickness, turning a block into a thin-walled shell.

---

## Overview

**Workbench:** Part Design · **Menu:** Part Design → Dress-up → Thickness  
**Toolbar:** Part Design Dress-up Features · **Shortcut:** none

Thickness takes an existing solid Body and applies a "thick solid" operation from the OpenCASCADE geometry kernel. You select one or more faces to open (remove), and the tool offsets all remaining faces by the specified thickness value to produce a hollow shell. The resulting solid has uniform wall thickness everywhere.

Thickness is a dress-up feature: it sits on top of another feature in the model tree and does not alter the original feature's parameters.

---

## Intuition

Imagine you have a machined aluminium block and you want to turn it into a hollow enclosure. You select the top face (which will become the open mouth) and enter a wall thickness of 2 mm. The tool keeps the outer shape exactly as it is and scoops out the interior, leaving 2 mm walls on all five remaining sides. The result looks exactly like a plastic injection-moulded box.

The **Reversed** option inverts the direction: instead of hollowing inward, material is added outward — like growing an outer shell around the original solid.

---

## When to use it

- You need a thin-walled enclosure or housing from a solid that was modelled as a full block.
- You want uniform wall thickness on an organic or complex shape without sketching each wall separately.
- You need to open multiple faces to create a shape with several apertures (e.g. a box open on two opposite sides).
- A sheet-metal-style body needs to be modelled in PartDesign for compatibility reasons.

---

## When NOT to use it

- **The shape has very thin features or near-zero-thickness regions** — OCCT's thick solid algorithm can fail on geometrically degenerate inputs. Consider modelling walls individually using Pad and Pocket instead.
- **You need variable wall thickness** — Thickness only supports a single uniform offset value. Use individual Pads, Pockets, or Lofts to control varying wall thicknesses.
- **You are working with sheet metal** — use the Sheet Metal workbench (an add-on) which provides bend, unfold, and k-factor operations appropriate for flat stock.
- **You want to shell a surface, not a solid** — Thickness is a solid operation. For surface offsets use the Part workbench Offset 3D Surface tool.

---

## Step-by-step walkthrough

**Prerequisites:** An active Body with a solid feature (Pad, Additive Box, Revolution, etc.) on top of the feature tree, and at least one flat or curved face to be opened.

1. **Select the face(s) to open.** Click one or more faces in the 3D view that you want to remove (the open faces of the resulting shell). Hold **Ctrl** to multi-select. You can also make no selection and add faces in the task panel.

2. **Activate Thickness.** Go to **Part Design → Dress-up → Thickness**. The task panel opens.

3. **Add or remove face references.** The **Select** button at the top of the task panel toggles selection mode. In selection mode, click faces in the viewport to add or remove them from the reference list. The reference list shows each face by its sub-element name (e.g. `Face1`).

4. **Set the thickness value.** Enter the wall thickness in the **Thickness** spinbox. The value must be positive and less than half the minimum dimension of the solid.

5. **Choose the Mode.** Select the offset computation mode from the **Mode** dropdown:
    - **Skin** (default) — offsets all non-selected faces inward (outward with Reversed), producing the classic hollow shell. Use this for most cases.
    - **Pipe** — produces a pipe-like shell by treating each face independently. May give unexpected results on convex shapes.
    - **Recto verso** — applies the offset symmetrically to both sides of each face (useful for plates and flanges).

6. **Choose the Join type.** Select how adjacent offset faces meet at edges:
    - **Arc** (default) — adds a fillet arc at concave corners where offset faces would otherwise intersect. Produces smooth, rounded interior corners.
    - **Intersection** — extends offset faces until they intersect and trims them. Produces sharp interior corners. Equivalent to mitre joins in sheet metal.

7. **Set Reversed.** Tick **Make thickness inwards** (the `Reversed` property) to apply the offset toward the interior of the solid. This is the normal use case for hollowing. Untick it to grow an outer shell around the original solid.

8. **Set Intersection.** Tick the **Intersection** checkbox to allow the offset algorithm to handle complex cases where offset faces of adjacent open faces would self-intersect. Leave it off for simple shapes.

9. **Click OK.** FreeCAD builds the thick solid and adds a Thickness feature to the model tree.

!!! tip
    If Thickness fails with an OCCT exception, try selecting fewer faces, reducing the thickness value, or switching the Join type from Arc to Intersection. Shells of complex solids sometimes require Intersection mode to resolve offset face collisions.

!!! tip
    Thickness respects the **Refine** option (inherited from `FeatureRefine`). If the resulting shell has redundant coplanar faces, tick **Refine** in the Properties panel to clean them up.

---

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Faces (Base sub-elements) | Face references list | — | The open faces of the result. Selected faces are removed; all remaining faces are offset by Value. |
| Thickness (Value) | Length (≥ 0) | 1.0 mm | Uniform wall thickness. Must be less than half the minimum solid dimension. |
| Mode | Enumeration: Skin / Pipe / Recto verso | Skin | OCCT offset mode. See deep dive below. |
| Join type | Enumeration: Arc / Intersection | Arc | How adjacent offset faces meet at interior edges. See deep dive below. |
| Reversed | Boolean | `True` | When `True`, thickness is applied toward the solid interior (hollow out). When `False`, material is added outward. |
| Intersection | Boolean | `False` | Enable OCCT intersection handling. Helps with shapes where offset faces would self-intersect at open face boundaries. |

### Mode in depth

The Mode dropdown selects the OCCT thick-solid algorithm. Each algorithm
handles the geometric problem of offsetting a shell differently, with
different trade-offs between robustness and behaviour at edges and corners.

#### Skin (default)

All faces of the closed shell are offset together as one connected surface,
preserving continuity between adjacent faces. The resulting inner shell is
a single connected object.

Intuition: imagine inflating a balloon inside the solid — the entire inner
surface inflates uniformly inward as one connected skin.

**Use this mode when:**
- The solid has a smoothly connected outer surface with no sharp re-entrant
  features (the common case: boxes, housings, brackets)
- You want the inner walls to remain tangent-continuous at convex corners

**Watch out for:** Skin is the most likely mode to fail with "Could not make
thick solid" on solids with complex topology (many short edges, very sharp
re-entrant corners, nearly-tangent faces). If Skin fails, try Intersection join
type first, then consider Pipe mode.

#### Pipe

Each face is offset independently, without maintaining continuity between
adjacent faces. The faces are then intersected or blended together to form the
inner shell.

Intuition: imagine peeling each panel of a cardboard box separately and then
gluing them back together — each face shifts on its own before the joins are
resolved.

**Use this mode when:**
- The solid has faces at very different angles and the Skin algorithm produces
  twisted or self-intersecting geometry at shared edges
- Diagnosing a Skin failure — Pipe is sometimes more robust on complex solids

**Watch out for:** Pipe can produce different inner-corner geometry than Skin.
The join quality at edges depends on the Join type setting and on how well the
offset faces intersect. On solids with many faces meeting at a point, Pipe can
also fail.

#### Recto verso

The original face is placed at the mid-plane of the resulting wall. The solid
offsets equally on both sides, so the wall is centred on the original surface.

Intuition: imagine the original surface is the neutral axis of a plate — material
is added equally to both sides so the plate is centred on where the surface was.

**Use this mode when:**
- Importing mid-surface geometry (e.g. thin-shell FEA models expressed as
  mid-surfaces) and converting it to a solid with physical thickness
- The design specifies wall thickness about a nominal surface, not measured from
  one face

**Watch out for:** This mode is uncommon in typical Part Design work. The
**Reversed** checkbox has no effect in Recto verso — the offset is always
symmetric. Total wall thickness = 2 × Value.

### Join type in depth

Join type controls how the inner offset faces meet at edges — the sharp corners
on the inside of the hollowed shell.

#### Arc (default)

At concave interior corners, a fillet arc is added to join adjacent offset faces
smoothly. The radius of the arc equals the wall thickness.

Intuition: the inside corner of the hollow is rounded, like the inside of a
bent plastic tube.

**Use this mode when:**
- The design is injection-moulded or cast, where sharp interior corners are
  stress concentrators and should be radiused
- A smooth interior surface is needed (cosmetic, flow simulation, or FEA)
- This is the standard choice for most plastic housings

**Watch out for:** Arc mode adds extra geometry (the fillet faces) at every
interior corner. This increases face count and can slow down downstream
operations. On very complex solids with many corners it may also fail — try
Intersection if Arc produces errors.

#### Intersection

Adjacent offset faces are extended until they intersect at a sharp edge, with
no rounding. The inside corners are crisp right-angle (or acute) edges.

Intuition: the inside corner of the hollow is sharp, like the inside corner of
a machined metal box.

**Use this mode when:**
- The design is machined metal or sheet metal, where inside corners are
  inherently sharp
- FEM meshing quality requires clean, sharp edges with no small-radius fillets
- Arc mode is producing errors or unexpected geometry

**Watch out for:** Very thin walls on a solid with a large open face can produce
Intersection joins that look correct but have zero-area faces (the intersection
is at an edge). Check the model carefully with a section view.

---

## Advanced usage

### Selecting multiple open faces

You can select more than one face as an open face. Each selected face is removed and all remaining faces are offset uniformly. For example, selecting both the top and bottom faces of a cylinder produces a hollow tube open at both ends.

When using multi-solid bodies, Thickness operates per-solid: each solid in the shape that has selected faces gets independently thickened, and the results are fused.

### Intersection mode for sharp corners

**Arc** mode adds material at concave interior corners to maintain a smooth wall. **Intersection** mode instead extends adjacent walls until they meet at a sharp edge. Intersection mode is faster and more robust on simple prismatic shapes. Use it when the design requires true right-angle interior corners (tooling considerations, FEM meshing quality, etc.).

### Recto verso mode for mid-plane walls

**Recto verso** is intended for plate-like solids where you want the original face to sit at the mid-plane of the resulting wall. The total wall thickness is `Value` and the original face is centred within it. This mode is uncommon in typical PartDesign workflows but useful when importing mid-surface geometry.

### Combining with other dress-ups

Thickness should generally come before Chamfer and Fillet in the model tree (i.e. apply chamfers and fillets after the shell is created), because OCCT's thick solid algorithm can struggle with already-rounded edges. If you need a chamfer on the opening edge, add it as a separate [Chamfer](chamfer.md) feature after the Thickness.

---

## Common mistakes and pitfalls

!!! warning "Failed to make thick solid"
    **Cause:** The OCCT thick solid kernel operation failed. Common causes: thickness is too large relative to the solid's minimum dimension, the selected face is adjacent to a very sharp edge, or two open faces share an edge.  
    **Fix:** Reduce the thickness value, switch from Arc to Intersection join type, deselect one of the faces, or simplify the solid geometry before applying Thickness. Check the Report View panel for the specific OCCT error message.

!!! warning "Invalid face reference"
    **Cause:** A face reference stored in the Thickness feature no longer corresponds to a face in the upstream solid (topology renaming after a recompute).  
    **Fix:** Double-click the Thickness feature to re-open its task panel, remove the stale reference, and re-select the correct face. Consider using the Subshape Binder to create stable face references.

!!! warning "Thickness produces an empty or inside-out shell"
    **Cause:** The **Reversed** checkbox is set incorrectly, or the thickness value exceeds the body dimensions.  
    **Fix:** Toggle **Make thickness inwards** (Reversed). If the shell is inside-out with Reversed on, the offset direction may have been interpreted differently by OCCT. Try toggling Reversed and observe which direction produces the hollow interior.

!!! warning "Result has extra unexpected faces"
    **Cause:** The offset algorithm added extra seam faces, or the solid had coplanar or near-coplanar faces before Thickness was applied.  
    **Fix:** Enable the **Refine** property on the Thickness feature (set it to `True` in the Properties panel). This calls OCCT's shape healing to remove redundant faces.

---

## Python API

### Minimal example

```python
import FreeCAD as App
import PartDesign

doc = App.newDocument()

body = doc.addObject("PartDesign::Body", "Body")

# Create a 10×10×10 box as the base feature
box = doc.addObject("PartDesign::AdditiveBox", "Box")
body.addObject(box)
box.Length = 10.0
box.Width  = 10.0
box.Height = 10.0
doc.recompute()

# Apply Thickness: open Face1 (the bottom face), 1 mm walls, inward
thickness = doc.addObject("PartDesign::Thickness", "Thickness")
body.addObject(thickness)
thickness.Base = (box, ["Face1"])  # Face1 is the bottom face of the box
thickness.Value = 1.0
thickness.Reversed = True          # hollow inward
thickness.Mode = 0                 # 0=Skin, 1=Pipe, 2=RectoVerso
thickness.Join = 0                 # 0=Arc, 1=Intersection
thickness.Intersection = False
doc.recompute()

print(f"Shell faces: {len(thickness.Shape.Faces)}")  # expects 11 faces
```

### Key properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Base` | `App::PropertyLinkSub` | — | The upstream feature and its open face sub-elements (e.g. `"Face1"`). Inherited from `DressUp`. |
| `Value` | `App::PropertyLength` | `1.0 mm` | Wall thickness. |
| `Mode` | `App::PropertyEnumeration` | `"Skin"` | OCCT offset mode: `"Skin"` (0), `"Pipe"` (1), `"RectoVerso"` (2). |
| `Join` | `App::PropertyEnumeration` | `"Arc"` | Corner join type: `"Arc"` (0), `"Intersection"` (1). (Note: internally index 1 maps to OCCT join type 2; Tangent join is not exposed in the UI.) |
| `Reversed` | `App::PropertyBool` | `True` | Apply offset inward (`True`) or outward (`False`). |
| `Intersection` | `App::PropertyBool` | `False` | Enable OCCT intersection handling at open face boundaries. |

---

## See also

- [Draft](draft.md) — add draft angles to faces for mould/cast removal
- [Fillet](fillet.md) — round the edges of the shell opening
- [Chamfer](chamfer.md) — bevel the edges of the shell opening
- [Pad](pad.md) — create the base solid that Thickness operates on
- [Additive Box](additive-box.md) — simple primitive solid for use as a Thickness base
