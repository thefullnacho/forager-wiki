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

**Edges** ([[ligaments]]):
- **Sells** the hardware [[forager-ml]] deploys to — the WALKING MAN PRO handheld; see [[edge-hardware]].
- Source of the [[brand-thesis]] + voice inherited by every other project's outward writing.
- **Planned:** supplies a "pest model" to [[hestia]] for HA pest alerts — VERIFY: no pest artifact
  exists yet; crop/companion-planting JSON is the likely raw material. See [[ligaments]].
