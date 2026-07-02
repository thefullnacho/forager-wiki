# homesteader-labs-site

`~/Documents/Forager/homesteader_labs_site_v01/Homesteader_labs/homesteader-labs-next-CLEAN`
· git `dev` · Next.js 14 (App Router)

**What it is:** the **Homesteader Labs** brand, content, and product site for off-grid
homesteaders — interactive survival/planting tools, a product catalog, and field-documentation
content. No backend DB: static JSON, MDX, and client-side localStorage. Has a strong per-repo
`CLAUDE.md` (defer to it for code) and the brand-voice files (`voice.md`, `newsletter-voice.md`,
`about-me.md`).

**Boundary:** the marketing + tools + content surface and the brand voice. Owns: crop database
(`content/crops/*.json`, incl. caloric + companion-planting data), product catalog
(`lib/products.ts` — WALKING MAN PRO, HELTEC V3), planting/survival calculators
(`lib/plantingIndex.ts`, `lib/survivalIndex.ts`), blog/archive MDX.

**Key buried logic (findable now — the point of this wiki):**
- **Pest emergence + companions:** `content/crops/pest-companions.json` — *phenology-aware*, not a
  static chart: each crop's `pests` carry `soilTempThreshold` + `gddThreshold` (emergence) and
  evidence-rated `companions` (trap-crop/repellent interplantings). Plus `companion-planting.json`.
  UI: `app/tools/caloric-security/companions/page.tsx`. This is the data the [[hestia]] pest-alert
  ligament consumes — see [[ligaments]].
- **Frost accuracy:** `lib/frostNormals.ts` → `getFrostDatesByZone(zone, zip)` over **NOAA
  1991–2020 normals** in `content/frost-zones.json` (USDA-zone keyed), fallback to a live
  `frost.date` API; `lib/zoneLookup.ts` resolves the zone; feeds `lib/plantingIndex.ts`.
- **Subsystems:** `lib/caloric-security/*` (yield/decay/energy/water autonomy scoring) and
  `lib/survivalPlan/*` (paid plan generator + Stripe). Defer to the code for mechanics.

**Edges** ([[ligaments]]):
- **Sells** the hardware [[forager-ml]] deploys to — the WALKING MAN PRO handheld; see [[edge-hardware]].
- Source of the [[brand-thesis]] + voice inherited by every other project's outward writing.
- **LIVE (2026-07-01):** supplies two datasets to [[hestia]] — `pest-companions.json` (pest-watch
  emergence alerts) and `frost-zones.json` (almanac frost normals), both vendored snapshots on
  hestia's side. The site stays the source of truth. See [[ligaments]].
