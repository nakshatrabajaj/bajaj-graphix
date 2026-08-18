---
name: new-design
description: Daily job — operator uploads a product mockup + customer branding; generate N design variants on the verified dieline. Use for any request to make carton/foil designs for a customer.
---

# Generate customer design variants

The operator uploads the product mockup EVERY job (compositions vary even on the same box size). Only the dieline geometry comes from the library.

## Step 1 — Gather the job inputs

1. **The mockup** — the plant's flat artwork for this product (CDR-PDF export and/or clear JPGs). Required every job; this is the source of all regulatory text.
2. **Box size** — L×H×W in mm (foil: paper + columns). Match it to a folder in `dielines/`. If no match: run `/new-dieline` first (one-time detour, needs plant verification).
3. **Brand name** for this product.
4. **Customer** — check `customers/`; if new, collect company name, marketed-by address, logo, and save them for reuse.
5. **Design references** (optional) and **number of variants** (default 5).

## Step 2 — Transcribe the mockup

Read every text block from the uploaded mockup verbatim: composition lines, colour, dosage, storage, warnings, caution, Mfg. Lic. No., manufacturer block, pack size, batch/MRP block incl. Hindi lines. These are regulatory — transcribe exactly, do not paraphrase. Flag anything unreadable to the operator instead of guessing.

## Step 3 — Build each variant

Start from the dieline's `dieline.svg` (copy its `<g>` geometry verbatim — never modify), then add:
- The transcribed fixed text in the standard layout (see `examples/aceclofenac-carton-master-example.svg` for the canonical panel layout and font sizes: generic name 16 pt bold, body ~5.4 pt, warning box, red CAUTION strip, WHO-GMP seal, batch block with Hindi).
- **Brand name**: 2 pt smaller than the generic name, brand color, under the generic name on front, back, and tuck flap.
- **Marketed-by block**: customer logo (vector), company, address, QR placeholder.
- **Design layer**: vector-only motifs/bands inside front/back/tuck panels; vary palette + motif family per variant; follow references if given. Never over regulatory text, never outside panels.

All text: plain `<text>`, Arial, whole strings, mm coordinates (1 pt = 0.3528 mm).

Optional concept round: AI-generated concept images may be shown for direction-picking only — never in the final SVG.

## Step 4 — Verify and deliver

1. Screenshot each variant; compare the transcribed text side-by-side against the uploaded mockup.
2. Overflow check (browser JS): every `<text>` bbox inside its panel — zero overflows.
3. Dieline `<g>` must be byte-identical to the library's dieline.svg.
4. Save to `jobs/YYYY-MM-DD-<brand>/`; send files (SendUserFile, attach). Remind: CorelDRAW File > Import; Hindi lines may need a Devanagari font swap.
5. Tell the operator to proofread the regulatory text against the original mockup before sending to the customer (transcription is high-fidelity but must be human-checked for pharma).
