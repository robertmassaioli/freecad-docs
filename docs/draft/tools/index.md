# Draft Tools

Complete index of all Draft workbench tools.

---

## Drafting — 2-D Creation

| Tool | Description |
|------|-------------|
| [Line](drafting.md#line) | Draw a single straight line segment |
| [Wire (Polyline)](drafting.md#wire-polyline) | Draw a connected sequence of line segments |
| [Fillet](drafting.md#fillet) | Create a fillet arc between two lines |
| [Arc](drafting.md#arc) | Draw a circular arc from centre, radius and angles |
| [Arc by 3 Points](drafting.md#arc-by-3-points) | Draw a circular arc through three points |
| [Circle](drafting.md#circle) | Draw a full circle from centre and radius |
| [Ellipse](drafting.md#ellipse) | Draw an ellipse or elliptical arc |
| [Rectangle](drafting.md#rectangle) | Draw a rectangle from two corner points |
| [Polygon](drafting.md#polygon) | Draw a regular polygon (3–n sides) |
| [B-Spline](drafting.md#b-spline) | Draw a B-spline curve through control points |
| [Cubic Bézier Curve](drafting.md#cubic-bezier-curve) | Draw a cubic Bézier curve with degree-3 segments |
| [Bézier Curve](drafting.md#bezier-curve) | Draw a Bézier curve of arbitrary degree |
| [Point](drafting.md#point) | Place a single point vertex |
| [Facebinder](drafting.md#facebinder) | Create a face from selected faces of 3-D objects |
| [ShapeString](drafting.md#shapestring) | Create a compound of text outlines as wires |
| [Hatch](drafting.md#hatch) | Fill a closed shape with an SVG or PAT hatch pattern |

---

## Annotation

| Tool | Description |
|------|-------------|
| [Text](annotation.md#text) | Place a multi-line text annotation |
| [Dimension](annotation.md#dimension) | Measure a distance, radius, or angle with a dimension line |
| [Label](annotation.md#label) | Place an annotated leader line with a callout text |
| [Annotation Style Editor](annotation.md#annotation-style-editor) | Create and manage named annotation style presets |

---

## Modification

| Tool | Description |
|------|-------------|
| [Move](modification.md#move) | Move or copy objects by a displacement vector |
| [Rotate](modification.md#rotate) | Rotate objects around an axis by an angle |
| [Scale](modification.md#scale) | Scale objects relative to a base point |
| [Mirror](modification.md#mirror) | Mirror objects across a line |
| [Offset](modification.md#offset) | Offset a wire or face by a given distance |
| [Trim / Extend](modification.md#trim-extend) | Trim or extend a wire to a boundary |
| [Stretch](modification.md#stretch) | Stretch objects by moving selected vertices |
| [Clone](modification.md#clone) | Create a linked clone of an object |
| [Edit](modification.md#edit) | Edit vertices or parameters of a Draft object interactively |
| [Subelement Highlight](modification.md#subelement-highlight) | Highlight a sub-element of an object for editing |
| [Join](modification.md#join) | Join two or more open wires into one |
| [Split](modification.md#split) | Split a wire at a selected point |
| [Upgrade](modification.md#upgrade) | Upgrade shapes to higher topology (edges → wire → face → solid) |
| [Downgrade](modification.md#downgrade) | Downgrade shapes to lower topology (solid → face → wire → edge) |
| [Wire to B-Spline](modification.md#wire-to-b-spline) | Convert a polyline to a B-spline |
| [Draft to Sketch](modification.md#draft-to-sketch) | Convert Draft objects to Sketcher objects or vice versa |
| [Slope](modification.md#slope) | Set the slope of a line or wire segment |
| [Flip Dimension](modification.md#flip-dimension) | Flip the orientation of a dimension text |
| [Shape 2D View](modification.md#shape-2d-view) | Project a 3-D shape onto the working plane as a 2-D view |

---

## Array Tools

| Tool | Description |
|------|-------------|
| [Ortho Array](arrays.md#ortho-array) | Create a 3-D rectangular (orthogonal) array |
| [Polar Array](arrays.md#polar-array) | Create a polar array around an axis |
| [Circular Array](arrays.md#circular-array) | Create concentric rings of copies |
| [Path Array](arrays.md#path-array) | Distribute copies along a path |
| [Path Link Array](arrays.md#path-link-array) | Like Path Array but using App::Link instances |
| [Point Array](arrays.md#point-array) | Place copies at positions defined by a point object |
| [Point Link Array](arrays.md#point-link-array) | Like Point Array but using App::Link instances |
| [Path Twisted Array](arrays.md#path-twisted-array) | Path array with twist rotation along the path |
| [Path Twisted Link Array](arrays.md#path-twisted-link-array) | Like Path Twisted Array using App::Link |

---

## Utilities

| Tool | Description |
|------|-------------|
| [Set Style](utilities.md#set-style) | Apply a named style preset to selected objects |
| [Apply Style](utilities.md#apply-style) | Apply the current style to selected objects |
| [Layer](utilities.md#layer) | Create a new layer |
| [Layer Manager](utilities.md#layer-manager) | Open the layer manager dialog |
| [Add Named Group](utilities.md#add-named-group) | Create a named group container |
| [Select Group](utilities.md#select-group) | Select all objects in a group |
| [Toggle Construction Mode](utilities.md#toggle-construction-mode) | Toggle construction geometry mode |
| [Add to Layer](utilities.md#add-to-layer) | Move selected objects to a layer |
| [Add to Group](utilities.md#add-to-group) | Add selected objects to a group |
| [Add to Construction](utilities.md#add-to-construction) | Move objects to the construction group |
| [Toggle Display Mode](utilities.md#toggle-display-mode) | Switch between Flat Lines and Wireframe display |
| [Toggle Grid](utilities.md#toggle-grid) | Show or hide the Draft grid |
| [Select Working Plane](utilities.md#select-working-plane) | Set the working plane |
| [Working Plane Proxy](utilities.md#working-plane-proxy) | Save the current working plane as a proxy object |
| [Heal](utilities.md#heal) | Attempt to repair malformed Draft objects |
| [Show Snap Bar](utilities.md#show-snap-bar) | Show the snap toolbar |

---

## Snapping

| Snap type | Description |
|-----------|-------------|
| [Snap Lock](snapping.md#snap-lock) | Master toggle for all snapping |
| [Endpoint](snapping.md#endpoint) | Snap to wire or edge endpoints |
| [Midpoint](snapping.md#midpoint) | Snap to the midpoint of an edge |
| [Center](snapping.md#center) | Snap to the centre of a circle or arc |
| [Angle](snapping.md#angle) | Snap to 45° increments on a circle or arc |
| [Intersection](snapping.md#intersection) | Snap to the intersection of two edges |
| [Perpendicular](snapping.md#perpendicular) | Snap to a point perpendicular to an edge |
| [Extension](snapping.md#extension) | Snap along the extension of an edge |
| [Parallel](snapping.md#parallel) | Snap along a direction parallel to an edge |
| [Special](snapping.md#special) | Snap to object-specific special points |
| [Near](snapping.md#near) | Snap to the nearest point on an edge |
| [Ortho](snapping.md#ortho) | Snap at 45° intervals from the last point |
| [Grid](snapping.md#grid) | Snap to grid intersections |
| [Working Plane](snapping.md#working-plane) | Project the snap point onto the working plane |
| [Dimensions](snapping.md#dimensions) | Show live dimension feedback while snapping |
