# claude-grass-seed-assistant.md — VCE Grass Seed Selection Assistant Project Context

Use this file to bring a new Claude session up to speed on this project.

---

## Project Overview

A single-file interactive HTML tool (`grass_seed_assistant.html`) that helps Virginia homeowners choose the right grass for their lawn. The user answers seven tappable questions about their site, use, and preferences; the tool recommends a VCE-adapted grass species or mixture, explains what to look for on the seed bag at the store, calculates how much to buy, and provides planting timing.

A separate lawn-alternative path guides users who want to replace turf with a native groundcover.

**Primary audience:** Chesterfield County, Virginia (Zone 7b, Southern Piedmont). Secondary: all of Virginia (five selectable regions).

**Companion tool:** The Lawn & Garden Care Calculator (`[LINK_TO_CALCULATOR]`) handles soil test interpretation and fertilizer/lime planning. The grass seed assistant links to it as a "next step" after planting. The two tools are independent — the grass seed assistant is question-driven (site/use/preference), not soil-test-driven.

No server, no build step — single self-contained HTML file. No external dependencies except Google Fonts CDN.

---

## Source Documents

### VCE Primary Sources

| Publication | Title | Used for |
| :-- | :-- | :-- |
| SPES-748 | "Selecting Turfgrass for Virginia" | Species properties, recommendation logic, region adaptation |
| SPES-617 | "Virginia Turfgrass Variety Recommendations" (updated yearly) | Certified cultivar backstop (collapsible) |
| 430-520 / SPES-223P | "Fall Lawn Care" | Renovation diagnostic framework, overseeding guidance |
| 430-522 / SPES-669P | Maintenance Calendar for Warm-Season Turfgrasses | Warm-season timing, N ceilings |
| 430-523 / SPES-670P | Maintenance Calendar for Cool-Season Turfgrasses | Cool-season timing |
| 426-609 | "Selecting Landscape Plants: Groundcovers" | Lawn-alternative path |
| EMG Handbook Ch. 11 | Lawn Establishment and Renovation | Diagnostic causes, groundcover guidance |
| EMG Handbook Ch. 19 | Virginia Native Plants | Native plant resources, VNPS approval |
| 456-018 / ENTO-634P | Pest Management Guide: Home Grounds & Animals | Herbicide/pesticide references (PMG selects product; label governs rate/timing) |

### Supplementary Sources

| Source | Used for |
| :-- | :-- |
| MSU Extension seed tag bulletin | Store-label literacy (seed analysis tag reading) |
| VCE 426-609 + MG guidance | Groundcover conversion steps |
| Plant Virginia Natives / Plant RVA Natives | Native plant lists and retail locator (Situation 1 — Chesterfield Extension campaign partner) |
| Virginia Native Plant Society (vnps.org) | Native plant lists and education (Situation 1 — listed in EMG Handbook Ch. 19) |
| USDA NRCS Plants Database (plants.usda.gov) | Native range confirmation (Situation 2) |
| Lady Bird Johnson Wildflower Center (wildflower.org) | Virginia-applicable native plant lists (Situation 5) |

**Source policy:** VCE always primary. Non-VCE sources follow the NonEdu source guidelines (see below). Never recommend brand names.

---

## File Structure

Single HTML file — all CSS, JS, HTML inline. Visual identity: VCE-green/earth palette, Fraunces/Newsreader/IBM Plex Mono typefaces (matches the Lawn & Garden Care Calculator).

### Key JavaScript Components

| Component | Purpose |
| :-- | :-- |
| `SPECIES` | 8 species/mixture property profiles from SPES-748 |
| `QUESTIONS` | 7 question definitions with options |
| `REGIONS` | 5 Virginia regions |
| `recommend(answers)` | Recommendation engine — SPES-748 logic |
| `DIAG` | 8 renovation diagnostic entries (VCE 430-520, EMG Ch. 11) |
| `renderQuestion()` | Renders the current question card |
| `renderResult()` | Renders the recommendation shopping card |
| `renderAlternative()` | Renders the lawn-alternative/groundcover path |
| `sampleTagSVG()` | Brand-free annotated sample seed tag (HTML) |
| `updateBuy(key)` | Seed quantity calculator (rate × area) |
| `fmtRate(arr)` | Formats seeding rate ranges |

---

## Recommendation Engine

### Question Flow (7 questions)

1. **Project** — new lawn / overseed / patch / lawn alternative (alternative short-circuits to `renderAlternative()`)
2. **Region** — dropdown, defaults to Southern Piedmont (incl. Chesterfield, Greater Richmond area)
3. **Sun** — full / part / shade / mixed
4. **Use** — active / light / low-input
5. **Look** — showcase / healthy / natural
6. **Maintenance** — high / moderate / low
7. **Winter color** — green year-round / dormant OK (auto-skips in Mountains — warm-season not adapted)

### Engine Logic (SPES-748)

- **Warm-season gate:** `wantsWarm = (winter === 'dormant') && warmRegion && (sun !== 'shade' || coastal)`
  - Warm regions: Southern Piedmont, Coastal Plain, Northern Piedmont
  - Shade stays cool-season everywhere except Coastal Plain (St. Augustine is shade-tolerant there)
- **Warm-season routing:** shade/part → St. Augustine (coastal only) or zoysia; low-input coastal → centipede; active → bermuda; default → zoysia
- **Cool-season routing:** shade → fine fescue; mixed → TF+KBG; full sun + high maintenance + showcase → TF+KBG; default → turf-type tall fescue

### Species Data (SPECIES object)

8 entries: `ttf` (turf-type tall fescue), `ttf_kbg` (tall fescue + Kentucky bluegrass mix), `fine` (fine fescues), `kbg` (Kentucky bluegrass), `prg` (perennial ryegrass), `zoysia`, `bermuda`, `centipede`, `staug` (St. Augustinegrass).

Each entry: `name`, `season`, `shade`, `traffic`, `drought`, `establish`, `winter`, `texture`, `maint`, `watch`, `rateNew`, `rateOver`, `timing`, `bag`, `mixNote`.

**Key rules baked in:**
- Cool-season = always a blend/mixture (never single cultivar)
- Warm-season = monoculture; several can't be seeded (St. Augustine = sod/plugs only; `rateNew: null`)
- Tall fescue is the statewide default for the cool-season path

---

## Store-Label Literacy

The "What to look for on the bag" checklist teaches generic, brand-free tag-reading:

- Read the **back analysis tag**, not the front (front = marketing, back = lab report). Tag location: small bags (15 lbs or less) printed on the back; larger bags often sewn into the top seam; jugs on the side panel.
- **Blue certification tag** — certifies varietal authenticity (not germination/purity).
- Named **cultivars/varieties**, not just species.
- **Germination 85% or higher** with **test date within 12 months** (germination drops about 5% a year).
- **Low inert matter** and near-zero weed seed / other crop seed.
- **Pure Live Seed** = pure-seed % × germination % (bag-comparison tool).
- **Coated-seed note** (in "How much to buy"): coating adds bag weight that isn't seed; rates assume uncoated seed.
- **Avoid:** Kentucky 31 (coarse pasture grass) and annual-ryegrass-heavy mixes (die within a year).

**Sample seed tag:** brand-free annotated HTML diagram in a boxed dropdown with green/red callouts.

---

## Renovation Diagnostic

For overseed and patch projects, an interactive troubleshooter shows before the recommendation:

8 tappable symptoms → likely cause + VCE fix. Plus a "see all causes and VCE fixes" expandable table.

| Symptom | Cause |
| :-- | :-- |
| Thin under trees / north side | Too much shade |
| Worn paths, pet runs, play areas | Traffic and soil compaction |
| Stays wet, puddles, grows moss | Poor drainage |
| Spongy feel; thick mat at the soil | Excess thatch |
| Never soil-tested; pale, weak growth | Fertility or pH problem |
| Dried out in summer; no irrigation | Drought / wrong grass |
| Spreading dead patches, spots, chewing | Insect or disease damage |
| Weeds crowded the grass out | Weed invasion |

Sources: VCE 430-520, EMG Handbook chapter 11, SPES-353.

**Weed and pest paths** reference the Pest Management Guide (456-018) for product selection and include "always follow the product label" (the label is legally binding and carries the planting-restriction interval).

---

## Lawn-Alternative Path

Triggered by Q1 option "Replacing lawn with a groundcover." Short-circuits to `renderAlternative()` (skips grass questions).

Content: VCE endorsement of groundcovers over turf where grass struggles (VCE 426-609, EMG Ch. 11); 5 good-swap scenarios; 7-step VCE/MG conversion process; native example (Pennsylvania sedge for dry shade); resources box.

### NonEdu Source Rules (applies to the resources box)

Non-educational sources follow a 5-situation approval framework. Currently approved for the lawn-alternative resources box:

| Source | Situation | Scope |
| :-- | :-- | :-- |
| Plant Virginia Natives / Plant RVA Natives | 1 (VCE-directed) | Native plant lists and retail locator only |
| Virginia Native Plant Society (vnps.org) | 1 (EMG Handbook Ch. 19 listing) | Native plant lists and education only — NOT range/nativity authority |
| USDA NRCS Plants Database (plants.usda.gov) | 2 (government) | Native range confirmation |
| Lady Bird Johnson Wildflower Center (wildflower.org) | 5 (university research center) | Virginia-applicable native plant lists |

**VNPS approval basis:** EMG Handbook Ch. 19 lists "Virginia Native Plant Society: https://vnps.org/" in Additional Resources, places it in the trusted set alongside .edu/.gov sources, and documents a Chesterfield-Extension-MG personnel link. The "agency partnership" argument was explicitly NOT the basis.

---

## Critical Implementation Rules

1. No backtick template literals — string concatenation only (Safari compatibility)
2. No Unicode/emoji in JS string literals — HTML entities only
3. No PII anywhere in code or comments
4. Never recommend brand names — species and properties only
5. Scientific (Latin) names use `<em>` (italics) — the only approved use of italics
6. Extension publication titles use quotation marks, not italics (VCE Style Guide)
7. Ranges use closed hyphens, not en dashes (VCE Style Guide)
8. Ampersands only in formal names; use "and" in editorial text
9. All list items end with a period (VCE Style Guide)
10. PMG selects product; product label governs rate, timing, and all other directions

---

## VCE Style Guide Compliance

Applied VCE Style Guide (VCE-1202, June 2025) on June 20, 2026. Guide priority order: VT Brand Guide → VCE Style Guide → AP Stylebook → Chicago Manual of Style.

**Required elements present:**
- Nondiscrimination statement (footer)
- AI disclosure statement (footer)
- "Virginia Cooperative Extension" spelled out on first mention; "VCE" / "Extension" on subsequent

**Style decisions applied:**
- Publication titles in quotation marks (not italics)
- Boldface for emphasis (not italics)
- Serial comma throughout
- Spaced em dashes for sentence breaks
- Closed hyphens for ranges
- Numbers one through nine spelled out (except measurements, percentages, dimensions)
- Percentages: figures + % symbol, no space
- "Chapter" spelled out (not "Ch.")
- Handbook/manual titles: no quotation marks (style guide excludes reference books)

---

## Attributions

Built from VCE sources: SPES-748 "Selecting Turfgrass for Virginia," the Extension Master Gardener Handbook chapter 11, and the lawn maintenance calendars (430-520/522/523). Variety details: VCE SPES-617 (updated yearly).

---

## Files

| File | Description |
| :-- | :-- |
| `grass_seed_assistant.html` | Main application |
| `claude-grass-seed-assistant.md` | Project context for AI-assisted development (this file) |

---

## Development History

### June 13, 2026 — Initial standalone build

Built as a self-contained HTML tool following four decisions:
1. **Build standalone first** (originally planned to port into the calculator as a tab; later made permanent — see June 20 entry).
2. **Lead with cultivar properties + store-label literacy**, not SPES-617 cultivar names. Residents can't buy SPES-617 varieties at a big-box store; SPES-617 is a collapsible backstop only.
3. **Include warm-season** as a real path.
4. **Default to Chesterfield (Southern Piedmont); others selectable.**

7 tappable questions with progress bar. Recommendation engine validated across scenarios. Region label: "Southern Piedmont — incl. Chesterfield, Greater Richmond area."

### June 14, 2026 — Print, diagnostic, tag diagram, refinements

- **Print feature** — "Print my shopping list" button; print stylesheet strips interactive elements, keeps only the recommendation card. Print-spacing fix (removed section-level `break-inside:avoid` that caused mostly-empty pages).
- **Renovation diagnostic** — 8-symptom interactive troubleshooter replacing static overseed note. Triggers for both overseed and patch projects.
- **Weed-control fix** — corrected against the PMG: start herbicide 30-45 days before renovation for re-treatment window. PMG number verified (456-018). "Follow the label" added to pesticide-relevant paths.
- **Store-label additions** — blue certification tag, top-seam tag location, test date within 12 months, coated-seed weight note, PLS as checklist item.
- **Sample seed-tag diagram** — brand-free annotated HTML in a boxed dropdown with green/red callouts.

### June 15, 2026 — Lawn-alternative path, VNPS approval

- **Lawn-alternative path** added (Q1 option "Replacing lawn with a groundcover"). NonEdu rule applied to sourcing; resources box rebuilt with rule-compliant sources.
- **VNPS approved** under Situation 1 (EMG Handbook Ch. 19 listing). Added to resources box with scope limit (native plant lists and education only, not range/nativity).

### June 20, 2026 — Standalone decision + VCE Style Guide compliance

- **Decision: stays standalone.** Will not be ported into the Lawn & Garden Care Calculator as a tab. Calculator reference changed from internal tab hand-off to a linked companion tool with placeholder URL.
- **VCE Style Guide compliance** — 12 changes across two passes: nondiscrimination statement, AI disclosure, publication titles (italics → quotation marks), en dashes → hyphens, ampersands → "and" in editorial text, terminal periods on list items, abbreviations expanded.
- **`claude-grass-seed-assistant.md` created** (this file) as standalone project context, extracted from the calculator's CLAUDE.md.
