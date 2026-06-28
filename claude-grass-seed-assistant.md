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
| SPES-299P / 424-100-C | "Agronomy Handbook Part III: Turfgrass" (Goatley, Hardiman, Chalmers, Whitt) | Per-species seeding rates in pure live seed (Table 1); seed-tag literacy; VCIA sod-quality standards (Table 5); annual ryegrass and Kentucky 31 (K-31) cautions; blue tag certification |
| SPES-617 / SPES-727 | "Virginia Turfgrass Variety Recommendations" (updated yearly) | Certified cultivar backstop (collapsible) |
| 430-520 / SPES-223P | "Fall Lawn Care" | Renovation diagnostic framework, overseeding guidance |
| 430-522 / SPES-669P | Maintenance Calendar for Warm-Season Turfgrasses | Warm-season timing, N ceilings |
| 430-523 / SPES-670P | Maintenance Calendar for Cool-Season Turfgrasses | Cool-season timing |
| 426-609 | "Selecting Landscape Plants: Groundcovers" | Lawn-alternative path |
| 418-040 | "Sod Source Selection, Installation, Maintenance, and Producers in Virginia" (Goatley) | Sod and plug shopping guidance for non-seedable species (St. Augustinegrass) |
| EMG Handbook Chapter 11 | Lawn Establishment and Renovation | Diagnostic causes, groundcover guidance, plug spacing |
| EMG Handbook Chapter 19 | Virginia Native Plants | Native plant resources, VNPS approval |
| 456-018 / ENTO-634P | Pest Management Guide: Home Grounds and Animals | Herbicide and pesticide references (PMG selects product; label governs rate, timing) |

### Land-Grant Extension Cooperators (preferred-tier hierarchy per Chesterfield Help Desk Source Guidelines, April 2026)

Used only where VCE does not cover the topic, with attention to climate parity. Preferred order: VCE → NC State → Clemson → UMD → Rutgers → Penn State → other.

| Source | Used for | Tier under the rule |
| :-- | :-- | :-- |
| NC State Extension publication AG-493, "Bermudagrass Production in North Carolina" | Existence of hulled, unhulled, and clay-coated bermudagrass seed forms; purpose of clay coating (broadcasting aid) | Preferred (NC State; climate-appropriate for Chesterfield County) |
| University of Maryland Extension publication FS759 ("Wildlife Plantings: Shrub Lespedezas") | Pure live seed conversion formula: lb. PLS recommended ÷ (% germination × % purity) = lb. of seed to plant | Preferred (UMD) |
| Texas A&M AgriLife Extension, Aggie Horticulture ("Bermudagrass") | Hulled-vs-unhulled planting biology — hulled for spring/summer germination speed, unhulled for late-fall/winter dormant seeding | "Other universities" tier (used because no preferred-tier source articulates the hulled-vs-unhulled timing distinction; biology is climate-independent; Virginia planting windows come from VCE Agronomy Handbook Part III, Table 2) |

### Other Supplementary Sources

| Source | Used for |
| :-- | :-- |
| VCE 418-040, "Sod Source Selection, Installation, Maintenance, and Producers in Virginia" | Listed under VCE Primary Sources above |
| Plant Virginia Natives / Plant RVA Natives | Native plant lists and retail locator (NonEdu Situation 1 — Chesterfield Extension campaign partner) |
| Virginia Native Plant Society (vnps.org) | Native plant lists and education (NonEdu Situation 1 — listed in EMG Handbook Chapter 19) |
| USDA NRCS Plants Database (plants.usda.gov) | Native range confirmation (NonEdu Situation 2 — federal government scientific agency) |
| Lady Bird Johnson Wildflower Center (wildflower.org) | Virginia-applicable native plant lists (NonEdu Situation 5 — university research center) |

### Sources Removed in This Pass

- **MSU Extension seed tag bulletin** — previously cited for store-label literacy. Removed because the Chesterfield Help Desk Source Guidelines (April 2026) place MSU in the "Other universities" tier (used only when no preferred-tier source covers the topic), and seed-tag literacy is now sourced directly to VCE Agronomy Handbook Part III (SPES-299P), with NC State and UMD as preferred-tier supplements.
- Various commercial seed seller references (Hancock Seed, Stover Seed, MBS Seed, Hay & Forage Magazine, Jacklin Seed newsletter) — never appeared in the deployed tool but were used as draft research for the warm-season seed labeling content; removed in favor of the Extension sources above per the rule's prohibition on "Commercial nursery or seed company websites."

**Source policy:** VCE always primary. Land-grant cooperators follow the preferred-tier hierarchy in the Chesterfield Help Desk Source Guidelines (April 2026). Non-.edu sources follow the five-situation NonEdu rules. Never recommend brand names. Every claim is footnoted to a numbered Source entry in the rendered result card.

---

## File Structure

Single HTML file — all CSS, JS, HTML inline. Visual identity: VCE-green/earth palette, Fraunces/Newsreader/IBM Plex Mono typefaces (matches the Lawn & Garden Care Calculator).

### Key JavaScript Components

| Component | Purpose |
| :-- | :-- |
| `SPECIES` | 8 species/mixture property profiles from SPES-748 |
| `SOURCES` | Master citation list keyed by short slug (e.g., `spes-748`, `ncsu-ag493`, `umd-pls`). VCE entries first, then land-grant cooperators per the Help Desk hierarchy |
| `cite(key)` | Inline citation helper — inserts a numbered superscript link and records the source in `citedOrder` |
| `citeMulti(keys)` | Same, for claims supported by multiple sources; renders each number as its own link |
| `renderSourcesBlock()` | Builds the Sources block from `citedOrder` at the end of each result; only sources actually cited in this specific recommendation appear |
| `QUESTIONS` | 7 question definitions with options |
| `REGIONS` | 5 Virginia regions |
| `recommend(answers)` | Recommendation engine — SPES-748 logic |
| `DIAG` | 8 renovation diagnostic entries (VCE 430-520, EMG Chapter 11) |
| `renderQuestion()` | Renders the current question card |
| `renderResult()` | Renders the recommendation shopping card; resets `citedOrder` and appends the Sources block at the end |
| `renderAlternative()` | Renders the lawn-alternative/groundcover path |
| `sampleTagSVG()` | Annotated sample seed tag modeled on VCE Agronomy Handbook Part III, p. 80 (SPES-299P) — uses VCE's example variety percentages and origin codes verbatim, with green/red pedagogical callouts overlaid |
| `updateBuy(key)` | Seed quantity calculator (rate × area, with hulled/coated multipliers for warm-season seed) |
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

Each entry: `name`, `season`, `shade`, `traffic`, `drought`, `establish`, `winter`, `texture`, `maint`, `watch`, `rateNew`, `rateOver`, `hasHulling` (warm-season seedable species only), `timing`, `bag`, `mixNote`.

**Key rules baked in:**
- Cool-season = always a blend/mixture (never single cultivar)
- Warm-season seedable species (bermuda, centipede) have non-null `rateNew` / `rateOver` and a `hasHulling` flag (true for bermuda only); the buy section renders hulled/coated toggles that adjust the calculated bag weight (×5 for unhulled bermuda per VCE Agronomy Handbook Part III Table 1; ×2 for coated as a typical-retail estimate, with UMD's PLS conversion formula offered for precision)
- St. Augustinegrass has null seeding rates (sod or plugs only); the bag section renders sod and plug shopping guidance instead, sourced to VCE 418-040
- Tall fescue is the statewide default for the cool-season path

## Store-Label Literacy

The "What to look for on the bag" checklist teaches generic, brand-free tag-reading, sourced primarily to VCE Agronomy Handbook Part III (SPES-299P):

- Read the **back analysis tag**, not the front (front = marketing, back = lab report). Tag location: small bags (15 lbs or less) printed on the back; larger bags often sewn into the top seam; jugs on the side panel.
- **Blue certification tag** — certifies varietal authenticity (per VCE Agronomy Handbook Part III: "ask for seed that is blue tag certified").
- Named **cultivars/varieties**, not just species (VCE SPES-617).
- **Germination 85% or higher** with **test date within 12 months** (germination drops about 5% a year).
- **Low inert matter** and near-zero weed seed / other crop seed.
- **Pure Live Seed** = pure-seed % × germination %; conversion formula `lb. PLS recommended ÷ (% germination × % purity) = lb. of seed to plant` (UMD Extension FS759).
- **Avoid:** Kentucky 31 (coarse pasture grass) and annual-ryegrass-heavy mixes (die within a year) — both sourced to VCE Agronomy Handbook Part III.

**For warm-season seed (bermuda, centipede), additional label items render in a sky-toned addendum:**

- **Hulled or unhulled** (bermuda only) — bermudagrass seed comes in three forms (hulled, unhulled, hulled-and-clay-coated) per NC State AG-493. Spring and summer plantings typically use hulled; late-fall and winter plantings sometimes use unhulled for dormant seeding (TAMU AgriLife).
- **Coating shows up as inert matter** — clay coating is added for easier broadcasting (NC State AG-493); read the inert-matter percentage to see what fraction of the bag is coating versus actual seed.
- **Compare bags by PLS, not bag weight** — VCE seeding rates are stated in PLS for this reason.

**Sample seed tag:** annotated HTML diagram modeled on VCE Agronomy Handbook Part III page 80. VCE's example variety percentages (49.11%, 34.63%, 9.95%, 4.94%), inert matter (1.37%), and origin codes are used verbatim; green and red pedagogical callouts are overlaid (not in the VCE original).

**Footnoting:** Every claim in the rendered result card is footnoted with a numbered superscript linking to a Sources block at the bottom of the card. The Sources block only lists sources actually cited in that specific recommendation, in citation order, per the Chesterfield Help Desk Source Guidelines: "the Sources section at the end should make it easy to see which source supports which recommendation."

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

Built primarily from VCE sources: SPES-748 "Selecting Turfgrass for Virginia," SPES-299P "Agronomy Handbook Part III: Turfgrass" (Goatley, Hardiman, Chalmers, Whitt), the Extension Master Gardener Handbook Chapter 11, and the lawn maintenance calendars (430-520/522/523). Variety details: VCE SPES-617/SPES-727 (updated yearly). Sod and plug shopping guidance: VCE 418-040 (Goatley).

Where VCE does not directly cover a topic, the tool draws from land-grant cooperator sources under the Chesterfield Help Desk Source Guidelines (April 2026): NC State Extension AG-493 "Bermudagrass Production in North Carolina" (hulled, unhulled, and clay-coated seed forms); University of Maryland Extension publication FS759 (pure live seed conversion formula); Texas A&M AgriLife Extension Aggie Horticulture "Bermudagrass" page (hulled-vs-unhulled planting biology, "Other universities" tier).

Every claim in the rendered result is footnoted with a numbered superscript and listed in a Sources block at the end of the recommendation, in citation order. Only sources actually cited in a given recommendation appear in that recommendation's Sources block.

---

## Files

| File | Description |
| :-- | :-- |
| `grass_seed_assistant.html` | Main application |
| `README.md` | User-facing project documentation (shown on GitHub repo page) |
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

### June 27, 2026 — Warm-season seedability, footnote system, and Help Desk Source Guidelines compliance

**SPECIES corrections (bermuda and centipede are seedable):**
- Bermuda `rateNew` and `rateOver` set to `[1, 2]` lbs PLS per 1,000 sq ft (VCE Agronomy Handbook Part III Table 1, hulled form); `hasHulling: true` added; `bag` and `mixNote` rewritten to distinguish common (seedable) from hybrid (sod/sprigs only — Tahoma 31, TifTuf, Latitude 36, etc.).
- Centipede `rateNew` and `rateOver` set to `[0.25, 0.5]` lbs per 1,000 sq ft (VCE Agronomy Handbook Part III Table 1); `hasHulling: false`.
- Previously both had `rateNew: null`, which caused the buy section to be skipped but the bag section to still render with cool-season-flavored content — an internal inconsistency surfaced by review.

**Buy section calculator — hulled and coated toggles:**
- For warm-season seedable species, the buy section now renders radio toggles for hulled/unhulled (bermuda only) and coated/uncoated. Multipliers applied: ×5 for unhulled (matches VCE Table 1's 5-10 vs. 1-2 lb spread); ×2 for coated as a typical-retail estimate (VCE doesn't address coating directly; UMD Extension's PLS-to-bag-weight formula is offered for precision).
- St. Augustinegrass remains `rateNew: null` and now gets a sod-and-plug shopping guidance section in place of the seed-bag content (VCE 418-040, EMG Chapter 11, SPES-617).

**Sample seed tag rebuilt to match VCE Agronomy Handbook Part III page 80:**
- Variety percentages (49.11%, 34.63%, 9.95%, 4.94%), inert matter (1.37%), and origin codes (OR, ID) match VCE's published example verbatim.
- Green and red pedagogical callouts retained as an overlay.
- Replaces the MSU Extension-derived layout used in the June 14 build.

**Numbered footnote system added:**
- `SOURCES` master citation object keyed by short slug (`spes-748`, `spes-299p`, `spes-617`, `430-520`, `430-522`, `430-523`, `emg-11`, `418-040`, `426-609`, `456-018`, `ncsu-ag493`, `umd-pls`, `tamu-bermuda`).
- `cite(key)` and `citeMulti(keys)` insert numbered superscript links and record use in `citedOrder`.
- `renderSourcesBlock()` builds a Sources block at the end of each result, listing only sources actually cited in this specific recommendation, in citation order, with the full citation and URL.
- All previous inline parenthetical citations ("(VCE 430-520)", "(VCE Agronomy Handbook Part III, SPES-299P, Table 1)") converted to numbered footnotes for consistency.
- Inline 456-018 references in DIAG entries for pest and weeds retained as in-text parenthetical attribution (clearest for the PMG-only pesticide rule); `showDiag()` also adds 456-018 to the Sources block when those entries are shown.

**Help Desk Source Guidelines (April 2026) compliance:**
- Audit found that earlier draft content had relied on commercial sources (Hancock Seed, Stover Seed, MBS Seed, Hay & Forage Magazine, Jacklin Seed newsletter) for the "coating is ~50% of bag weight" claim and the hulled-vs-unhulled biology explanation — all prohibited under the rule's "Commercial nursery or seed company websites" exclusion.
- Replaced with: VCE Agronomy Handbook Part III (SPES-299P) for seeding rates and PLS framing; NC State Extension AG-493 for the existence and purpose of hulled/unhulled/coated forms; UMD Extension FS759 for the PLS-to-bag-weight conversion formula; TAMU AgriLife Extension for the hulled-vs-unhulled planting biology (used under the "Other universities" tier, with attention to climate parity — biology is climate-independent).
- The "~50% of bag weight" specific claim was dropped and replaced with "read the inert-matter percentage on your bag's tag" — the inert-matter figure is the tag-derived truth, not an asserted industry-wide number.
- The ×2 coated multiplier in the calculator is now described as "a typical estimate for retail coated bags," not as a sourced claim, with the UMD PLS formula offered for precision.
- MSU Extension seed-tag bulletin removed from supplementary sources (covered by preferred-tier sources now).

**README.md updated:**
- Warm-season grass table now distinguishes seedable common bermudagrass from sod/sprig hybrid cultivars (Tahoma 31, TifTuf, Latitude 36).
- Store-Label Literacy section gained a "Warm-season seed — extra label items" subsection covering hulled/unhulled and coating heaviness.
- Sources section restructured into two tables: VCE primary (now including SPES-299P and 418-040) and Land-Grant Extension Cooperators (NC State AG-493, UMD FS759, TAMU AgriLife with tier explanation).
- Technical Details bullet added for the numbered footnote system and the Help Desk Source Guidelines compliance.
- Print section updated to note the Sources block prints alongside the rest.
- Style cleanups: "Ch. 11" → "chapter 11"; "Home Grounds & Animals" → "Home Grounds and Animals"; "+" → "and" in region descriptions; "sod/plugs" → "sod or plugs".

### June 20, 2026 — Standalone decision + VCE Style Guide compliance

- **Decision: stays standalone.** Will not be ported into the Lawn & Garden Care Calculator as a tab. Calculator reference changed from internal tab hand-off to a linked companion tool with placeholder URL.
- **VCE Style Guide compliance** — 12 changes across two passes: nondiscrimination statement, AI disclosure, publication titles (italics → quotation marks), en dashes → hyphens, ampersands → "and" in editorial text, terminal periods on list items, abbreviations expanded.
- **`claude-grass-seed-assistant.md` created** (this file) as standalone project context, extracted from the calculator's CLAUDE.md.
