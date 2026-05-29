# Feeds and Speeds

> **In one sentence:** Feeds and speeds are the two most critical cutting
> parameters — spindle RPM controls how fast the tool rotates; feed rate
> controls how fast it moves through the material — and choosing them
> correctly prevents tool breakage, poor surface finish, and wasted material.

---

## The two fundamental parameters

**Spindle speed (RPM)** — how many revolutions per minute the cutter makes.
Too slow: rubbing instead of cutting, built-up edge, poor finish. Too fast:
thermal damage, tool breakage.

**Feed rate (mm/min)** — how fast the tool moves through the material.
Too slow: re-cutting chips, work hardening (metals), heat build-up. Too fast:
tool deflection, breakage, chatter.

The two parameters are linked: increasing RPM while holding feed rate
constant means each cutting edge takes a smaller chip (lower chip load).
Decreasing RPM while holding feed rate constant means larger chips (higher
chip load). The target is a chip load in the manufacturer's recommended range.

---

## Calculating spindle speed (RPM)

RPM is set by the desired **surface speed** (also called cutting speed —
the speed of the cutter's edge as it passes through the material):

```
RPM = (Vc × 1000) / (π × D)

where:
  Vc  = surface speed  (m/min)
  D   = tool diameter  (mm)
  π   ≈ 3.1416
```

Simplified to a practical form:

```
RPM ≈ (Vc × 318.3) / D
```

### Recommended surface speeds by material

| Material | Surface speed Vc (m/min) | Notes |
|----------|------------------------|-------|
| Aluminium (6061) | 200–500 | Use coolant or air blast; carbide preferred |
| Aluminium (cast) | 150–350 | More abrasive; reduce speed |
| Mild steel (1018) | 30–60 | Requires coolant; HSS or carbide |
| Stainless steel (304) | 20–40 | Work-hardens — maintain constant feed |
| Titanium | 15–30 | Coolant essential; carbide required |
| Hardwood | 100–200 | Dry; sharp tool critical |
| Softwood / MDF | 150–300 | Dry; chip-load larger than hardwood |
| HDPE | 150–400 | Dry or mist; avoid melting chips |
| Acrylic / PMMA | 100–250 | Sharp tools; allow chips to clear |
| Brass | 80–200 | Dry; free-machining alloys at the higher end |
| Carbon fibre (CFRP) | 100–250 | Dust extraction essential; wear PPE |

!!! warning "Always verify against your tool manufacturer's data"
    These are starting values. The correct speed depends on tool coating,
    number of flutes, machine rigidity, coolant strategy, and depth of cut.
    Indexed insert tooling can run 2–5× faster than solid carbide figures above.

### Example: 6 mm end mill in 6061 aluminium

```
Vc = 300 m/min (mid-range for aluminium)
D  = 6 mm
RPM = (300 × 318.3) / 6 = 15,915 RPM
```

Most hobby CNC routers run 10,000–24,000 RPM; the calculated value fits.

---

## Calculating feed rate

Feed rate is set by the desired **chip load** — the thickness of material
each cutting edge removes per revolution:

```
feed_mm_min = RPM × chip_load_mm × number_of_flutes
```

### Recommended chip loads by material

| Material | Chip load (mm) per flute |
|----------|-------------------------|
| Aluminium (6061) | 0.03–0.08 (3 mm tool) to 0.08–0.15 (12 mm tool) |
| Mild steel | 0.01–0.04 |
| Stainless steel | 0.008–0.03 |
| Hardwood | 0.03–0.10 |
| Softwood / MDF | 0.05–0.15 |
| HDPE | 0.04–0.12 |
| Acrylic | 0.03–0.08 |

Chip load increases with tool diameter. A small tool requires a smaller chip
load to avoid breakage; a large tool can take a bigger chip.

### Example: 6 mm 2-flute end mill in 6061 aluminium

```
RPM       = 15,915 (from above)
chip_load = 0.05 mm (conservative for a 6 mm tool in aluminium)
flutes    = 2
feed      = 15,915 × 0.05 × 2 = 1,592 mm/min  ≈ 1600 mm/min
```

---

## Plunge feed rate (VertFeed)

The plunge feed rate (Z-axis downward movement into the material) is
typically **25–50 % of the horizontal feed rate**:

```
VertFeed ≈ feed_mm_min × 0.3
```

Centre-cutting end mills can plunge directly; non-centre-cutting tools
require a ramped entry — use a [Ramp Entry dress-up](../tools/dressup-ramp-entry.md).

---

## Radial and axial engagement

The formulas above assume full-width radial engagement (slot milling).
Reducing engagement multiplies the effective feed rate:

| Radial engagement (% of diameter) | Feed rate multiplier |
|------------------------------------|---------------------|
| 100 % (slotting) | × 1.0 (base values) |
| 50 % | × 1.3–1.5 |
| 25 % | × 2.0–2.5 |
| 10 % | × 3.0–4.0 |

[Adaptive Clearing](../tools/adaptive-clearing.md) automatically maintains
a constant engagement angle, which is why it can run at significantly higher
feed rates than conventional pocketing.

---

## Setting feeds and speeds in FreeCAD

FreeCAD does **not** auto-calculate feeds and speeds. Values must be entered
manually in the [Tool Controller](../tools/tool-controller.md):

| Field | Enter |
|-------|-------|
| `HorizFeed` | Horizontal feed rate (mm/min) |
| `VertFeed` | Plunge feed rate (mm/min) |
| `SpindleSpeed` | RPM |
| `SpindleDir` | CW for most end mills |

!!! warning "SpindleSpeed defaults to 0"
    A new Tool Controller has `SpindleSpeed = 0`. The post-processor will
    write `S0` in the G-code — the machine spindle will not start. Always
    set a non-zero RPM before post-processing.

---

## Common problems and their causes

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Tool breaks immediately | Feed too high or plunge too fast | Reduce feed; use ramp entry |
| Chatter / vibration | RPM too low or feed too high | Increase RPM; reduce feed; shorten tool stick-out |
| Poor surface finish (rough) | Chip load too high (chips tearing) | Reduce feed or increase RPM |
| Melting chips (plastics) | RPM too high | Reduce RPM; clear chips actively |
| Rapid tool wear | Surface speed too high | Reduce RPM |
| Rubbing, built-up edge | Surface speed too low | Increase RPM |
| Work hardening (stainless) | Feed too low — tool rubbing | Increase feed; maintain constant engagement |

---

## See also

- [Tool Controller](../tools/tool-controller.md) — where feeds/speeds are set per job
- [Operation Parameters](operation-parameters.md) — depths, heights, step-down
- [Tool Bit Files](tool-bit-files.md) — tool geometry definitions
- [Adaptive Clearing](../tools/adaptive-clearing.md) — high-efficiency milling strategy
- [Dressup: Ramp Entry](../tools/dressup-ramp-entry.md) — ramped plunge for non-centre-cutting tools
- [CAM Workbench](../index.md) — workbench overview
