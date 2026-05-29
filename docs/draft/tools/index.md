# Draft Tools

Complete index of all Draft workbench tools.

---

## Drafting — 2-D Creation

| Tool | Description |
|------|-------------|
| [Line](line.md) | Draw a single straight line segment |
| [Wire (Polyline)](wire.md) | Draw a connected sequence of line segments |
| [Fillet](fillet.md) | Create a fillet arc between two lines |
| [Arc](arc.md) | Draw a circular arc from centre, radius and angles |
| [Arc by 3 Points](arc-3points.md) | Draw a circular arc through three points |
| [Circle](circle.md) | Draw a full circle from centre and radius |
| [Ellipse](ellipse.md) | Draw an ellipse or elliptical arc |
| [Rectangle](rectangle.md) | Draw a rectangle from two corner points |
| [Polygon](polygon.md) | Draw a regular polygon (3–n sides) |
| [B-Spline](bspline.md) | Draw a B-spline curve through control points |
| [Cubic Bézier Curve](bezier-cubic.md) | Draw a cubic Bézier curve with degree-3 segments |
| [Bézier Curve](bezier-curve.md) | Draw a Bézier curve of arbitrary degree |
| [Point](point.md) | Place a single point vertex |
| [Facebinder](facebinder.md) | Create a face from selected faces of 3-D objects |
| [ShapeString](shapestring.md) | Create a compound of text outlines as wires |
| [Hatch](hatch.md) | Fill a closed shape with an SVG or PAT hatch pattern |

---

## Annotation

| Tool | Description |
|------|-------------|
| [Text](text.md) | Place a multi-line text annotation |
| [Dimension](dimension.md) | Measure a distance, radius, or angle with a dimension line |
| [Label](label.md) | Place an annotated leader line with a callout text |
| [Annotation Style Editor](annotation-style-editor.md) | Create and manage named annotation style presets |

---

## Modification

| Tool | Description |
|------|-------------|
| [Move](move.md) | Move or copy objects by a displacement vector |
| [Rotate](rotate.md) | Rotate objects around an axis by an angle |
| [Scale](scale.md) | Scale objects relative to a base point |
| [Mirror](mirror.md) | Mirror objects across a line |
| [Offset](offset.md) | Offset a wire or face by a given distance |
| [Trim / Extend](trim-extend.md) | Trim or extend a wire to a boundary |
| [Stretch](stretch.md) | Stretch objects by moving selected vertices |
| [Clone](clone.md) | Create a linked clone of an object |
| [Edit](edit.md) | Edit vertices or parameters of a Draft object interactively |
| [Subelement Highlight](subelement-highlight.md) | Highlight a sub-element of an object for editing |
| [Join](join.md) | Join two or more open wires into one |
| [Split](split.md) | Split a wire at a selected point |
| [Upgrade](upgrade.md) | Upgrade shapes to higher topology (edges → wire → face → solid) |
| [Downgrade](downgrade.md) | Downgrade shapes to lower topology (solid → face → wire → edge) |
| [Wire to B-Spline](wire-to-bspline.md) | Convert a polyline to a B-spline |
| [Draft to Sketch](draft-to-sketch.md) | Convert Draft objects to Sketcher objects or vice versa |
| [Slope](slope.md) | Set the slope of a line or wire segment |
| [Flip Dimension](flip-dimension.md) | Flip the orientation of a dimension text |
| [Shape 2D View](shape-2d-view.md) | Project a 3-D shape onto the working plane as a 2-D view |

---

## Array Tools

| Tool | Description |
|------|-------------|
| [Ortho Array](array-ortho.md) | Create a 3-D rectangular (orthogonal) array |
| [Polar Array](array-polar.md) | Create a polar array around an axis |
| [Circular Array](array-circular.md) | Create concentric rings of copies |
| [Path Array](array-path.md) | Distribute copies along a path |
| [Path Link Array](array-path-link.md) | Like Path Array but using App::Link instances |
| [Point Array](array-point.md) | Place copies at positions defined by a point object |
| [Point Link Array](array-point-link.md) | Like Point Array but using App::Link instances |
| [Path Twisted Array](array-path-twisted.md) | Path array with twist rotation along the path |
| [Path Twisted Link Array](array-path-twisted-link.md) | Like Path Twisted Array using App::Link |

---

## Utilities

| Tool | Description |
|------|-------------|
| [Set Style](set-style.md) | Apply a named style preset to selected objects |
| [Apply Style](apply-style.md) | Apply the current style to selected objects |
| [Layer](layer.md) | Create a new layer |
| [Layer Manager](layer-manager.md) | Open the layer manager dialog |
| [Add Named Group](add-named-group.md) | Create a named group container |
| [Select Group](select-group.md) | Select all objects in a group |
| [Toggle Construction Mode](toggle-construction-mode.md) | Toggle construction geometry mode |
| [Add to Layer](add-to-layer.md) | Move selected objects to a layer |
| [Add to Group](add-to-group.md) | Add selected objects to a group |
| [Add to Construction](add-to-construction.md) | Move objects to the construction group |
| [Toggle Display Mode](toggle-display-mode.md) | Switch between Flat Lines and Wireframe display |
| [Toggle Grid](toggle-grid.md) | Show or hide the Draft grid |
| [Select Working Plane](select-working-plane.md) | Set the working plane |
| [Working Plane Proxy](working-plane-proxy.md) | Save the current working plane as a proxy object |
| [Heal](heal.md) | Attempt to repair malformed Draft objects |
| [Show Snap Bar](show-snap-bar.md) | Show the snap toolbar |

---

## Snapping

| Snap type | Description |
|-----------|-------------|
| [Snap Lock](snap-lock.md) | Master toggle for all snapping |
| [Endpoint](snap-endpoint.md) | Snap to wire or edge endpoints |
| [Midpoint](snap-midpoint.md) | Snap to the midpoint of an edge |
| [Center](snap-center.md) | Snap to the centre of a circle or arc |
| [Angle](snap-angle.md) | Snap to 45° increments on a circle or arc |
| [Intersection](snap-intersection.md) | Snap to the intersection of two edges |
| [Perpendicular](snap-perpendicular.md) | Snap to a point perpendicular to an edge |
| [Extension](snap-extension.md) | Snap along the extension of an edge |
| [Parallel](snap-parallel.md) | Snap along a direction parallel to an edge |
| [Special](snap-special.md) | Snap to object-specific special points |
| [Near](snap-near.md) | Snap to the nearest point on an edge |
| [Ortho](snap-ortho.md) | Snap at 45° intervals from the last point |
| [Grid](snap-grid.md) | Snap to grid intersections |
| [Working Plane](snap-working-plane.md) | Project the snap point onto the working plane |
| [Dimensions](snap-dimensions.md) | Show live dimension feedback while snapping |
