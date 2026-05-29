# Layer

> **In one sentence:** Creates a new layer in the document — a visual style
> container that controls colour, line width, and draw style for all member objects.

---

## Overview

**Workbench:** Draft  
**Menu:** Draft → Utilities → Layer  
**Command:** `Draft_Layer`

Layers appear in the model tree under a "Layers" container. Objects assigned
to a layer inherit the layer's visual properties. Changing the layer updates
all members at once.

---

## Layer properties

| Property | Description |
|----------|-------------|
| Line Color | Line colour for all layer members |
| Line Width | Line width for all layer members |
| Draw Style | Solid, Dashed, Dotted, DashDot |
| Shape Color | Fill colour for layer member faces |
| Transparency | Transparency for layer member faces |
| Visibility | Show/hide all objects on the layer at once |

---

## Python API

```python
import Draft

layer = Draft.make_layer(
    name="Walls",
    line_color=(0.0, 0.0, 0.0, 1.0),
    shape_color=(0.8, 0.8, 0.8, 1.0),
    line_width=1.0,
    draw_style="Solid",
    transparency=0
)
App.ActiveDocument.recompute()
```

---

## Common mistakes and pitfalls

!!! warning "Layer colour overrides object colour"
    When an object belongs to a layer, the layer's colour takes precedence
    over the object's own colour. If changing an object's colour in the
    Properties panel has no effect, check whether it is on a layer.

---

## See also

- [Layer Manager](layer-manager.md) — manage all layers from one panel
- [Add to Layer](add-to-layer.md) — assign objects to a layer
- [Draft Workbench](../index.md) — workbench overview
