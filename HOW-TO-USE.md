# How to use this system (for operators)

You make packaging design mockups for customers using Claude. Everything happens inside this folder.

## One-time setup (per person)

1. Install the Claude Code desktop app and sign in (each operator needs a Claude account).
2. Open THIS folder (`bajaj-graphix`) in Claude Code. That's it — the rules load automatically.

## Daily work: making designs for a customer

Type `/new-design` and give Claude:

1. **The product's mockup file** — upload it every time (the .cdr PDF from the plant, or clear photos of the flat artwork). Claude reads all the medicine text from it.
2. **The box size** — length × height × width in mm (get it from the plant if you don't know it)
3. **The customer's brand name** (e.g. BIDOL-FORTE)
4. **The customer's company name, address, and logo** (customers already saved are reused — just say the name)
5. **Design references** the customer likes (optional)
6. **How many designs** (usually 5)

Claude makes the designs, checks them, and gives you SVG files. Each file:
- Opens in CorelDRAW with File > Import — all wording is editable there
- Has the exact correct sizes and mark lines — never changed
- Goes to the customer for approval, then to the plant

**IMPORTANT: proofread the medicine text** (composition, warnings, license numbers) against the original mockup before sending anything to a customer. Claude copies it carefully, but medicine text must always be human-checked.

## If Claude says the box size is new

That size isn't in the library yet. Claude will walk you through `/new-dieline`:
give it the exact measurements, then ask the plant head to import the dieline file
into CorelDRAW ONCE and confirm the measurements. After that, every future product
using that box size works instantly. (There are only a handful of box sizes, so
this stops happening quickly.)

## Rules Claude follows automatically (you don't need to remember these)

- Sizes and mark lines are NEVER changed — they come from the die tooling
- Brand name is always 2 pt smaller than the medicine name
- All legal/regulatory text is copied exactly from your uploaded mockup
- Output is always editable SVG (not flat images)

## If something looks wrong

- Text not editable in CorelDRAW → tell Claude, it will fix the file format
- Hindi text shows wrong font in CorelDRAW → normal; select it and switch to a Hindi font (e.g. Mangal)
- Wrong size → this should never happen; tell Claude AND the plant head immediately
