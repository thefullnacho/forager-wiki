# homesteader-labs-site

`~/Documents/Forager/homesteader_labs_site_v01/Homesteader_labs/homesteader-labs-next-CLEAN`
· deploy branch `master` · Next.js 16 (App Router) *(corrected 2026-09-30; was `dev` / 14)*

**What it is:** the **Homesteader Labs** brand, content, and product site for off-grid
homesteaders — interactive survival/planting tools, a product catalog, and field-documentation
content. No backend DB: static JSON, MDX, and client-side localStorage. Has a strong per-repo
`CLAUDE.md` (defer to it for code) and the brand-voice files (`voice.md`, `newsletter-voice.md`,
`about-me.md`).

**Boundary:** the marketing + tools + content surface and the brand voice. Owns: crop database
(`content/crops/*.json`, incl. caloric + companion-planting data), product catalog
(`lib/products.ts` — WALKING MAN PRO only; the HELTEC V3 listing was a stale claim, corrected 2026-09-30 per the repo `CLAUDE.md`), planting/survival calculators
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
- **2026-09-30:** the build loop. The site's audience is makers ([[brand-thesis]]); build logs
  get their own newsletter signup tagged `source = builds`, and [[hestia]] hardware becomes the
  subject of measured posts, starting with a greenhouse door board. PLANNED edge, see [[ligaments]].
- **Sells** the hardware [[forager-ml]] deploys to — the WALKING MAN PRO handheld; see [[edge-hardware]].
- Source of the [[brand-thesis]] + voice inherited by every other project's outward writing.
- **LIVE (2026-09-20):** publishes its reference tables as public JSON endpoints with an OpenAPI
  spec, licensed per dataset (CC BY 4.0 for the pest compilation, public-domain-derived for zone
  and frost). First move of the agent-readiness bet: be early, be the resource an assistant reaches
  for. Metering is deferred on purpose, since the public-domain-derived parts are rebuildable by
  anyone and the defensible dataset, observed days-to-maturity, does not exist yet. See
  [[ligaments]].
- **2026-09-17:** first attempt to read the [[hestia]] edge backwards, pulling homestead soil
  telemetry as evidence for a post. Rejected on the data, not the idea; see [[ligaments]]. The
  site's own writing now treats sensor readings as observation, not measurement.
- **2026-09-17:** transcribes and tonemaps its own post videos on the [[dev-box-and-cuda]] box with
  hestia's faster-whisper service environment; video is self-hosted because `/privacy` forbids the
  cookies a YouTube embed sets.
- **LIVE (2026-07-01):** supplies two datasets to [[hestia]] — `pest-companions.json` (pest-watch
  emergence alerts) and `frost-zones.json` (almanac frost normals), both vendored snapshots on
  hestia's side. The site stays the source of truth. See [[ligaments]].
