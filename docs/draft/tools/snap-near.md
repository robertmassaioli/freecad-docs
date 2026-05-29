# Snap Near

> **In one sentence:** Snaps to the nearest point on any edge, regardless
> of whether it is an endpoint, midpoint, or anywhere along the edge.

---

## Overview

**Workbench:** Draft  
**Toolbar:** Draft Snap  
**Command:** `Draft_Snap_Near`

**Indicator:** Yellow dot sliding along the edge.

The least selective snap — it finds the closest position on any edge.

---

## Common mistakes and pitfalls

!!! warning "Near overrides more specific snaps"
    Near tends to override Endpoint and Midpoint snaps. Disable it when you
    want to pick specific features, and only enable it when you need to attach
    to an arbitrary edge position.

---

## See also

- [Snap Endpoint](snap-endpoint.md) — exact endpoints (more selective)
- [Snap Lock](snap-lock.md) — master snap toggle
- [Draft Workbench](../index.md) — workbench overview
