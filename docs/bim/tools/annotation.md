# Annotation and Documentation

BIM annotation tools add dimensions, labels, section planes, and
construction documentation to the model. Most produce objects that are
visible in both the 3-D view and on TechDraw sheets.

---

## Text {#text}

**Menu:** Annotation → Text  
**Command:** `BIM_Text`

Places a 3-D text annotation at a point in the model. The text is a
`Draft::Text` object. It scales with the working plane and can be moved
and rotated like any object.

---

## Dimensions {#dimensions}

BIM provides three aligned dimension tools:

| Tool | Command | Description |
|------|---------|-------------|
| **Dimension (Aligned)** | `BIM_DimensionAligned` | Measures along the line between two points |
| **Dimension (Horizontal)** | `BIM_DimensionHorizontal` | Measures the horizontal component only |
| **Dimension (Vertical)** | `BIM_DimensionVertical` | Measures the vertical component only |

Dimensions are `Draft::Dimension` objects. They display the measured length
with a unit suffix and are drawn as leader lines in the 3-D view.

### Step-by-step

1. Activate a dimension tool.
2. Click the first measurement point.
3. Click the second measurement point.
4. Click where to place the dimension text.
5. The dimension is created.

Dimensions can reference vertices, edge midpoints, or free points.

---

## Leader {#leader}

**Menu:** Annotation → Leader  
**Command:** `BIM_Leader`

Creates a leader line — an arrow pointing to a model feature, with a text
label at the far end. Used for notes, part numbers, and callouts.

---

## Axis {#axis}

**Menu:** Annotation → Axis  
**Command:** `Arch_Axis`  
**IFC class:** `IfcGridAxis`

Creates a construction axis line (a reference datum in one direction). Axes
are used as column-grid references. They appear as dashed lines labelled A,
B, C… or 1, 2, 3…

---

## Axis System {#axis-system}

**Menu:** Annotation → Axis System  
**Command:** `Arch_AxisSystem`

Combines two or three `Arch_Axis` objects into a 2-D or 3-D column grid.
The grid points (intersections of axes) can be used to place columns and
other repetitive elements.

---

## Grid {#grid}

**Menu:** Annotation → Grid  
**Command:** `Arch_Grid`  
**IFC class:** `IfcGrid`

Creates a 2-D rectangular grid object. Unlike the Draft working plane grid,
this Grid is a persistent document object with specified rows, columns, and
spacing.

---

## Section Plane {#section-plane}

**Menu:** Annotation → Section Plane  
**Command:** `Arch_SectionPlane`

Creates an `Arch::SectionPlane` — a cutting plane that generates 2-D orthographic
views (plan, section, elevation) from the 3-D model.

### How it works

1. Place the section plane at the desired cut elevation or orientation.
2. Select the objects the section should include in its `Objects` property.
3. Use **Create 2D Views → Shape 2D View** (or **Drawing View**) to
   extract a 2-D projection from the section plane.
4. The projection can be placed on a TechDraw page.

### Key properties

| Property | Description |
|----------|-------------|
| `Objects` | List of objects included in the section |
| `OnlySolids` | Cut only solid objects (ignore meshes, wires) |
| `Clip` | If True, clip all geometry to the plane extent |
| `CutMargin` | Extra depth behind the cut plane to include |

---

## Drawing Page {#drawing-page}

**Menu:** Annotation → Drawing Page  
**Command:** `BIM_TDPage`

Creates a new TechDraw page using a standard architectural sheet template
(A0–A4, with title block). This is the same as TechDraw's Page command but
pre-selects a BIM-appropriate template.

---

## Drawing View {#drawing-view}

**Menu:** Annotation → Drawing View  
**Command:** `BIM_TDView`

Adds a view of a Section Plane's cut geometry to the active TechDraw page.
The view includes:
- Cut lines (the cross-section at the cut plane)
- Visible edges behind the cut plane
- Hidden lines (if enabled)

### Step-by-step

1. Create a Section Plane and set its `Objects`.
2. Create a Drawing Page with `BIM_TDPage`.
3. Select the Section Plane, then run **Annotation → Drawing View**.
4. The 2-D view appears on the TechDraw page.

---

## Common mistakes and pitfalls

!!! warning "Section Plane shows no geometry"
    **Cause:** The `Objects` property of the Section Plane is empty, or the
    objects are not within the plane's extent.  
    **Fix:** Set `Objects` to include all the elements you want sectioned,
    and ensure the plane intersects them.

!!! warning "Dimensions appear very small or very large"
    **Cause:** Dimensions scale with the working-plane pixel size. At
    building scale (metres), the default font size is tiny.  
    **Fix:** Set the `FontSize` and `ArrowSize` properties in the annotation
    style, or use the Annotation Style Editor to create a building-scale style.

---

## See also

- [Building Elements](elements.md) — the objects you annotate
- [IFC Management](ifc.md) — property sets and schedules
