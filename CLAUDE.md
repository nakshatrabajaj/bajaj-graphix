# Bajaj Graphix — AI Design Mockup System

This project generates **pharma packaging design mockups** (cartons and blister foils) for customers of Bajaj Graphix / Bajaj Formulations. An operator uploads a product mockup + customer branding, and Claude produces editable vector design files the plant can open in CorelDRAW.

## Architecture: dielines are the library, mockups come per job

One box size serves MANY products/compositions, so the library stores **dieline geometry only** (`dielines/`). The product mockup — with its composition and regulatory text — is uploaded fresh with every job and transcribed onto the verified dieline.

- **`/new-design`** — the daily job: mockup upload + brand + customer + refs → N design variants. Outputs to `jobs/`.
- **`/new-dieline`** — one-time per box SIZE: register exact dimensions in `dielines/`, plant head verifies once in CorelDRAW.

## HARD RULES — never violate these

1. **The dieline is untouchable.** Panel sizes, flap sizes, cut/crease mark lines come from the plant's die-cutting tooling. Copy the library dieline's `<g>` verbatim into every design. Never redraw, move, resize, restyle, add, or drop dieline geometry.
   - **Mark-line style (user requirement, 18 Aug 2026):** mark lines are drawn plant-style — long crossing LINES that overshoot each corner/junction by ~3 mm, exactly like the CDR mockups — never closed rectangles with neat corners. In finished designs the dieline `<g>` is placed LAST in the file so the lines paint ON TOP of the design layer and stay visible.
   - **Bleed:** replicate the uploaded mockup — wherever its design crosses a border line, the generated design layer crosses the same line the same way (reference measurements: ~3 mm past cut edges and into flap folds, ~1.5 mm across internal panel folds, kept clear of regulatory text). Bleed is ink overflow only; the mark lines themselves never move.
2. **Never estimate dimensions.** If exact measurements (carton L×H×W; foil paper width + column repeats) are not provided, STOP and ask. Do not derive sizes from images.
3. **Flap height = box width.** Tuck flap and bottom flaps fold over to become the top/bottom faces (L×W), so their height equals the box WIDTH (e.g. 117×72×70 carton → 70 mm flaps).
4. **Brand name is 2 pt smaller than the composition (generic) name**, in the brand color, under the generic name on front, back, and tuck-flap panels.
5. **Output = editable SVG only.** Plain `<text>` elements (whole strings, never per-character), `font-family="Arial"`, mm coordinates: width/height in mm, viewBox numerically equal to mm (1 pt font = 0.3528 mm). White background rect. Verified 14 Aug 2026: CorelDRAW imports this with fully retypeable text (SVG beats PDF for this).
6. **Never use AI image generators in the final master** — flat pixels don't belong in print files. Vector motifs only. Generated images are allowed solely as concept references.
7. **Regulatory text is transcribed verbatim** from the uploaded mockup — composition, storage, warnings, CAUTION strip, Mfg. Lic. No., manufacturer block, WHO-GMP seal, batch/MRP block, Hindi statutory lines. Never paraphrase; flag unreadable text instead of guessing. Operator must proofread before customer delivery.
8. **.cdr cannot be written directly.** The plant imports the SVG (File > Import) and saves as .cdr. Hindi lines may need a Devanagari font swap in Corel — expected, not a bug.

## Folder structure

- `dielines/<type>-<LxHxW>/` — `dieline.svg` (frozen geometry) + `spec.json` (dimensions, panel map, verified_by_plant flag).
- `customers/<slug>/` — `info.json` + `logo.svg` for repeat customers.
- `examples/` — canonical layout reference (aceclofenac master example, AI TEST finished design) + original plant source files.
- `jobs/YYYY-MM-DD-<brand>/` — generated variants per job.

## Verification (every generated file, before delivery)

1. Render + screenshot; sanity-check layout and compare transcribed text against the uploaded mockup.
2. Overflow check: every `<text>` bbox inside its panel (zero tolerance).
3. Dieline `<g>` byte-identical to the library's dieline.svg.
4. Deliver with SendUserFile (display: attach); remind the operator to proofread regulatory text.
