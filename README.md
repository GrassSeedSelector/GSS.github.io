# 🌱 Which Grass Should I Plant?

**A Virginia Cooperative Extension grass selection tool for homeowners**

Answer seven quick questions about your site, and get a VCE-adapted grass recommendation, what to look for on the seed bag at the store, and how much to buy.

> Built from VCE turfgrass publications. No brands, no ads — just Extension science in a tap-friendly format.

---

## 🎯 What This Tool Does

A Virginia homeowner walks into a big-box store, stares at a wall of seed bags, and has no idea which one to grab. This tool gets them ready *before* they go.

**In about two minutes, the tool delivers:**

- 🌾 **A grass recommendation** matched to their region, sunlight, use, and maintenance level — powered by VCE SPES-748 logic
- 🛒 **What to look for on the bag** — a brand-free checklist that teaches how to read a real seed analysis tag
- 📐 **How much to buy** — enter the lawn area, get the pounds
- 📅 **When to plant** — species-specific timing windows
- 🔍 **Renovation diagnosis** (for overseed/patch projects) — interactive troubleshooter: tap the symptom, get the VCE fix
- 🌼 **Lawn alternative path** — for sites where a native groundcover beats turf

---

## 🗺️ How It Works

```
 ┌──────────────┐
 │  7 questions  │──→ Site, sun, use, look, maintenance, winter color
 └──────┬───────┘
        ▼
 ┌──────────────┐
 │   Engine     │──→ SPES-748 logic: cool-season vs. warm-season routing
 └──────┬───────┘
        ▼
 ┌──────────────────────────────────────────┐
 │  Shopping card                           │
 │  ├─ Recommended grass + why it fits      │
 │  ├─ Property profile (shade, traffic…)   │
 │  ├─ Bag checklist + sample seed tag      │
 │  ├─ Seed calculator (rate × area)        │
 │  ├─ Planting window                      │
 │  └─ Next step → Lawn & Garden Calculator │
 └──────────────────────────────────────────┘
```

---

## 🌿 Grasses Covered

### ❄️ Cool-Season (green year-round)

| Grass | Best for | Shade | Traffic |
| :-- | :-- | :-- | :-- |
| **Turf-type tall fescue** | All-around home lawn (Virginia's default) | Moderate | Good |
| **Tall fescue + KBG mix** | High-traffic showcase lawns in full sun | Moderate | Excellent |
| **Fine fescues** | Deep shade, low-input areas | Best | Poor |
| **Kentucky bluegrass** | Full-sun showcase with high maintenance | Poor | Excellent |
| **Perennial ryegrass** | Quick cover, mix component | Moderate | Good |

### ☀️ Warm-Season (dormant tan in winter)

| Grass | Best for | Region limit | Seeded? |
| :-- | :-- | :-- | :-- |
| **Zoysiagrass** | Dense, drought-tough lawns | Piedmont + Coast | Some types |
| **Bermudagrass** | High-traffic, full-sun lawns | Piedmont + Coast | Yes |
| **Centipedegrass** | Lowest-input warm-season | Far south only | Yes |
| **St. Augustinegrass** | Shade-tolerant warm-season | SE coast only | No — sod/plugs |

---

## 🏷️ Store-Label Literacy

The heart of the tool. Instead of recommending a brand, it teaches the homeowner *how to evaluate any bag on the shelf*:

- 📋 **Read the analysis tag**, not the front of the bag (front = marketing; tag = lab report)
- 🏷️ **Blue certification tag** — certifies varietal authenticity
- ✅ **Named cultivars** — not just the species
- 🔬 **Germination 85%+** with a test date within 12 months
- ⚖️ **Pure Live Seed** — the real comparison metric between two bags
- 🧪 **Low inert matter**, near-zero weed seed
- ⚠️ **Avoid** Kentucky 31 (pasture grass) and annual ryegrass (dies in a year)
- 🎨 **Coated-seed trap** — coating adds bag weight that isn't seed

A **brand-free sample seed tag** with annotated green/red callouts shows exactly what each line means.

---

## 🔍 Renovation Diagnostic

For overseeding or patching projects, the tool asks *why did it fail?* before recommending a grass — because reseeding won't fix a site problem.

Eight tappable symptoms (shade, traffic, drainage, thatch, soil, drought, pest, weeds) each produce a likely cause and VCE-sourced fix. Pesticide-relevant paths reference the VCE Pest Management Guide (456-018) and include "always follow the product label."

---

## 🌼 Lawn-Alternative Path

When the user picks "Replacing lawn with a groundcover," the tool skips the grass questions and delivers:

- ✅ Five scenarios where a groundcover beats turf
- 🪴 A seven-step VCE/Master Gardener conversion process
- 🌿 A native example (Pennsylvania sedge for dry shade)
- 📚 Approved native-plant resources (VCE publications, Plant Virginia Natives, Virginia Native Plant Society, USDA NRCS, Lady Bird Johnson Wildflower Center)

---

## 🖨️ Print

The "Print my shopping list" button produces a clean one- to two-page printout: just the recommendation card, bag checklist, and seed quantity — ready to take to the store.

---

## 🗂️ Virginia Regions

The tool covers five regions, defaulting to Southern Piedmont (Chesterfield, Greater Richmond area):

| Region | Notes |
| :-- | :-- |
| Northern Mountains | Warm-season grasses not adapted; winter-color question auto-skips |
| Southern Mountains | Same as above |
| Northern Piedmont | Warm-season available when dormancy is acceptable |
| **Southern Piedmont** | Default — includes Chesterfield, Richmond metro |
| Coastal Plain | Warmest zone; St. Augustinegrass adapted here |

---

## 📖 Sources

All recommendations trace to Virginia Cooperative Extension publications:

| Publication | Title |
| :-- | :-- |
| SPES-748 | "Selecting Turfgrass for Virginia" |
| SPES-617 | "Virginia Turfgrass Variety Recommendations" (updated yearly) |
| 430-520 | "Fall Lawn Care" |
| 430-522 | Maintenance Calendar for Warm-Season Turfgrasses |
| 430-523 | Maintenance Calendar for Cool-Season Turfgrasses |
| 426-609 | "Selecting Landscape Plants: Groundcovers" |
| EMG Handbook Ch. 11 | Lawn Establishment and Renovation |
| 456-018 | Pest Management Guide: Home Grounds & Animals |

---

## ⚙️ Technical Details

- **Single HTML file** — all CSS, JS, and HTML inline; no build step, no server
- **No external dependencies** except Google Fonts CDN (Fraunces, Newsreader, IBM Plex Mono)
- **No backtick template literals** — string concatenation only (Safari compatibility)
- **No brand names** anywhere in the code or output
- **Print stylesheet** included; interactive elements stripped in print
- **Responsive** — single-column layout on all screen widths

---

## 🤝 Companion Tool

Once the lawn is planted, the **[Lawn & Garden Care Calculator]([LINK_TO_CALCULATOR])** handles the next phase: soil test interpretation, fertilizer planning, and lime application for cool-season lawns, warm-season lawns, vegetable gardens, flower gardens, and shrubs.

---

## 📄 Attributions

During the preparation of this tool the author(s) used Claude (Anthropic) in order to assist with coding, content drafting, and structuring the interactive interface. After using this tool, the author(s) reviewed and edited the content as needed and take(s) full responsibility for the content of the publication.

Virginia Cooperative Extension is a partnership of Virginia Tech, Virginia State University, the U.S. Department of Agriculture (USDA), and local governments, and is an equal opportunity employer. For the full nondiscrimination statement, please visit ext.vt.edu/accessibility.
