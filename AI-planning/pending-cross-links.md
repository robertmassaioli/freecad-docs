# Pending Cross-Workbench Links

**Status:** Tracking file  
**Purpose:** Record every cross-workbench (or cross-concept) link that was
temporarily removed or replaced with plain text during the initial documentation
sprint. When the target page is written, restore these as proper Markdown links.

---

## How to use this file

For each entry below:

1. Write the target page.
2. Find the source file listed under **Source file**.
3. Replace the plain-text mention with the hyperlink shown under **Restore as**.
4. Delete the entry from this file.
5. Run `mkdocs build --strict` and confirm 0 warnings.

---

## Part Design concept pages (not yet written)

These pages were referenced by multiple tool pages but were never created.
The links were redirected to the closest existing page as a temporary fix.
When these concept pages are written, restore the original links.

### `concepts/body-and-origin.md`

This page should explain:
- The Part Design Body container: what it is, why it exists, how features
  chain inside it.
- The built-in Origin (XY, XZ, YZ planes and X, Y, Z axes) that every Body
  automatically contains.
- The Tip: what it is, how to move it, why the order of features matters.

Once written, restore the following redirected links:

| Source file | Section | Current link | Restore as |
|-------------|---------|--------------|------------|
| `boolean.md` | See also | `[Body](body.md)` | `[Body and Origin](../concepts/body-and-origin.md)` |
| `datum-line.md` | See also | `[Body](body.md)` | `[Body and Origin](../concepts/body-and-origin.md)` |
| `datum-plane.md` | See also | `[Body](body.md)` | `[Body and Origin](../concepts/body-and-origin.md)` |
| `mirrored.md` | See also | `[Datum Plane](datum-plane.md)` | `[Body and Origin](../concepts/body-and-origin.md)` with text "the built-in planes available as mirror references" |
| `move-feature-in-tree.md` | See also | `[Body](body.md)` | `[Body and Origin](../concepts/body-and-origin.md)` |
| `move-feature.md` | See also | `[Body](body.md)` | `[Body and Origin](../concepts/body-and-origin.md)` |
| `move-tip.md` | See also | `[Body](body.md)` | `[Body and Origin](../concepts/body-and-origin.md)` |

### `concepts/attachment.md`

This page should explain:
- The `Part::AttachExtension` framework that all datum objects and primitives
  use for positioning.
- All available attachment modes (Deactivated, Translate origin, Object's X Y Z,
  Plane face, etc.) with descriptions.
- How to read the Attachment dialog, set a reference, and choose a mode.

Once written, restore the following redirected links:

| Source file | Section | Current link | Restore as |
|-------------|---------|--------------|------------|
| `additive-box.md` | Parameters | `[Datum Plane](datum-plane.md)` | `[Attachment](../concepts/attachment.md)` |
| `subtractive-box.md` | Parameters | `[Datum Plane](datum-plane.md)` | `[Attachment](../concepts/attachment.md)` |
| `subtractive-cone.md` | Parameters | `[Datum Plane](datum-plane.md)` | `[Attachment](../concepts/attachment.md)` |
| `subtractive-cylinder.md` | Parameters | `[Datum Plane](datum-plane.md)` | `[Attachment](../concepts/attachment.md)` |
| `subtractive-sphere.md` | Parameters | `[Datum Plane](datum-plane.md)` | `[Attachment](../concepts/attachment.md)` |

Note: The same pattern likely applies to the additive primitives
(additive-cone, additive-cylinder, additive-sphere, additive-ellipsoid,
additive-torus, additive-prism, additive-wedge, subtractive-ellipsoid,
subtractive-torus, subtractive-prism, subtractive-wedge). Check those files
for `<!-- TODO: verify link -->` comments pointing to attachment.md and
restore them at the same time.
