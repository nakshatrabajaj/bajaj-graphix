---
name: new-dieline
description: Register a new box/foil SIZE in the dieline library — one-time per size. Use when a design job arrives with dimensions that don't match any folder in dielines/.
---

# Register a new dieline (one-time per size)

The dieline library stores GEOMETRY only — box sizes and mark-line layouts. One dieline serves every product/composition that uses that box size.

## Step 1 — Collect (ask; never guess)

- Carton: exact length × height × width in mm, from the plant.
- Foil: exact paper width in mm + column repeat widths in mm (e.g. 225 mm paper, 45/28/42 columns).
- Any non-standard flap/panel arrangement (default assumption: reverse-tuck carton, flap height = box width).

If dimensions are missing, STOP and ask the operator. Never derive from images.

## Step 2 — Build

Create `dielines/<type>-<LxHxW>/`:
- `dieline.svg` — white base + one `<g>` of dieline rects (`stroke="#999999" stroke-width="0.2"`), mm-exact, viewBox = mm. Panel layout for a reverse-tuck carton: end panel (W×H) | front (L×H) | composition (W×H) | back (L×H) | glue flap (6×H); dust flap (L×8) + tuck flap (L×W) above front; bottom flaps (L×W) under front and back. **Flap height = box width.**
- `spec.json` — dimensions, canvas size, full panel x/y map, `"verified_by_plant": false`.

## Step 3 — Verify

1. Render + screenshot; check the layout.
2. Ask the operator to have the plant head import dieline.svg into CorelDRAW once and measure the panels. When confirmed, set `"verified_by_plant": true` in spec.json.
