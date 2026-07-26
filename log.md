# Log

Append-only. One dated line per ingest / decision / lint pass. Newest at the bottom.

- **2026-06-23 — wiki seeded.** Scaffolded the constellation wiki ([[CLAUDE]] schema, [[index]],
  [[ligaments]], 4 project pages, 4 entity pages). Members: [[hestia]], [[forager-ml]],
  [[forager-field-station]], [[homesteader-labs-site]]. Seeded from a read of all four repos.
  - Recorded DIVERGENCE: field-station serves 3 experts (psychedelics omitted for optics);
    forager_ml trains 4 and still has a populated `psychedelics_dataset` — the believed
    "cleanup to match" has NOT happened. See [[model-registry]].
  - Flagged VERIFY: field-station router = `domain_router_v2` vs forager_ml manifest
    `domain_router` (v1); and a param-count doc drift (~9M vs ~4.9M).
  - Flagged TODO: the [[hestia]] ← pest-alert ligament has no source artifact yet (no `pest`
    model/dataset on the site; crop/companion-planting JSON is the candidate raw material).
  - Context: created right after hestia's voice phase wrapped (browser mic + the `libcublas`
    LD_LIBRARY_PATH fix), which surfaced the shared CUDA gotcha now in [[dev-box-and-cuda]].
- **2026-06-23 — ingest: located the site's pest/frost logic; pinned the pest-alert source.**
  Dug through the marketing site libs (it was "buried"). Found `content/crops/pest-companions.json`
  is a *phenology-aware* pest-emergence table (soil-temp + GDD thresholds, evidence-rated
  companions), not a static chart — and it's keyed to exactly what [[hestia]] already senses
  (per-bed soil temp/GDD). Upgraded the [[ligaments]] pest-alert edge from "no source" to that
  concrete data source, and inventoried the buried logic (pest, frost `lib/frostNormals.ts`,
  caloric-security / survivalPlan subsystems) on [[homesteader-labs-site]]. Pointer commits to the
  four repos landed (hestia & field-station on new `docs/forager-wiki-pointer` branches; forager_ml
  & site on `dev`).
- **2026-06-23 — decision: wiki doubles as an Obsidian vault (read/navigate lens).** The repo
  already speaks Obsidian natively (`[[wikilinks]]`, flat markdown, basename link resolution), so
  no migration — just open the folder as a vault. Write path stays git + agent; Obsidian is for
  reading/graph/backlinks only. Added `.gitignore` (`.obsidian/` + OS cruft) so per-machine GUI
  state can't fork the LLM-maintained content. Local `.obsidian` config tuned to match the schema:
  wikilinks kept (`useMarkdownLinks: false`), `newLinkFormat: shortest` (basename), and
  `alwaysUpdateLinks: false` + daily-notes/templates/note-composer disabled so Obsidian never
  auto-rewrites links or auto-creates notes. Graph view stands in as a live lint dashboard for
  [[ligaments]] edges and unresolved `[[links]]`.
- **2026-06-23 — added the first playbook: [[brag-video]].** New `playbooks/` category for repeatable
  cross-project workflows. Captures the `/brag` → Hyperframes pipeline and the gotchas from the
  Hestia run (Outfit-only fonts, audio needs ids, GSAP hard-kills, timeline capping, beat-sync) so
  the next crank (field-station / forager_ml / site) is faster. Both Hestia cuts logged as templates.
- **2026-06-23 — second [[brag-video]] template: the [[homesteader-labs-site]] field-terminal cut.**
  Ran the playbook on the marketing site → a cinematic terminal piece (bold `#ff7300` on black,
  monospace, HUD brackets, typewriter hook) built from the site's real copy. Logged as **template B**
  (vs Hestia's polished **template A**) so future cranks pick a tone by *brand*, not house style.
  The playbook paid off: faster run (reused hyperframes via symlink), and the only surprise was the
  pre-warned font rule (`Caveat` not auto-resolved → mono). Output kept in a sibling dir, not the
  site's Next.js repo.
- **2026-06-23 — market research landed in [[hestia]] (`~/hestia/MARKET.md`).** External/competitive
  intel (kept in-repo, not copied up — the wiki holds the pointer, not the content). Frames Hestia as
  a four-way convergence play (self-host local-LLM + persistent memory + home control + household
  records) with no competitor holding the center: Khoj has memory but no house; Nabu Casa's HA Voice
  PE has the house but a thin/loop-owning brain; big-tech Alexa+/Gemini own neither ownership nor
  your memory. Rides the 2026 "local AI hub" tailwind (17k+ HA users on local STT/LLM, Qwen3
  consensus = Hestia's lineage). **Constellation-relevant finding:** *Mind the Farm* (talk-to-your-
  homestead-records SaaS) validates demand for the planned sensing→homestead ligament — Forager
  pest/plant ID → HA alerts surfacing through Hestia's `records` tool. See [[ligaments]].
- **2026-06-28 — [[hestia]] went public + first Show HN.** First public push of the constellation:
  Hestia open-sourced under **AGPL-3.0** at github.com/thefullnacho/hestia (clean `main` branch;
  private `master` keeps the full dev history). Launched as a **Show HN** ("a local-first home
  assistant that trusts timers over the LLM") — the constellation's first real discovery push. The
  lead hook is the [[brand-thesis]] *decloudify / local-first, no-cloud* pillar (runs a local LLM on
  your own box, nothing exposed to the internet), so the brand framing carries straight into the
  product pitch. AGPL chosen deliberately: copyleft keeps it open even when run as a network
  service, while asking nothing of home self-hosters (leaves dual-licensing open later).
- **2026-06-29 — [[hestia]] voice goes to real hardware: HA Voice PE kitchen satellite live.**
  The room mic the [[hestia-phase3-voice]] staging waited on is onboarded and working end-to-end,
  **fully local**: "Okay Nabu" (on-device wake) → faster_whisper → `conversation.hestia` (qwen3:14b)
  → piper, spoken back in the kitchen — chose *Full Local Processing*, declined HA Cloud. Notable
  cross-cutting point: it runs on **Nabu Casa's own Voice PE hardware but with Hestia's brain**, which
  is the live embodiment of the [[brand-thesis]] decloudify pillar and exactly the wedge `MARKET.md`
  named (Nabu owns the house + the hardware but ships a thin/loop-owning brain → Hestia drops its
  own brain into that same hardware). Setup gotchas (HA `internal_url` was advertising the Tailscale
  name to a LAN-only device; red ring = hardware mute) are repo/ops detail — kept in-repo + hestia's
  `memory/`, not copied up here per schema.
- **2026-07-01 — new shared entity [[anti-slop-principle]].** Named the constellation-wide "model
  on a short leash" stance and linked it as the *engineering* sibling of [[brand-thesis]]'s "abstain
  over bluff." Same tenet, per-repo flavor: [[hestia]] determinism-over-intelligence (grounding not
  recall, deterministic skill routing, eval-backed 14B-over-30B), [[forager-ml]]/[[forager-field-station]]
  refuse-when-unsure (deadly-mushroom veto), [[homesteader-labs-site]] the outward framing. Added to
  [[index]]; back-linked from [[brand-thesis]] and [[hestia]]. Prompted by framing the defense to
  "isn't Hestia just AI slop orchestration with a chatbox" — the answer *is* this principle.
- **2026-07-01 — first data ligaments go LIVE: [[homesteader-labs-site]] → [[hestia]] ×2.** The
  pest-alert edge shipped exactly as pinned on 2026-06-23 (*data, not a model*): hestia vendors
  the site's `pest-companions.json` and runs a fully deterministic GDD spine — biofix = last
  spring frost *observed* in the Open-Meteo archive (2026: Apr 21), soil temp estimated from
  trailing air temp, one alert per pest per season on the 7am garden push. Mid-season first run
  marked 20 already-open windows silently instead of flooding. A second, unplanned ligament
  landed the same day: `frost-zones.json` (the site's NOAA 1991-2020 normals) now feeds hestia's
  new **almanac** — a nightly-regenerated season page (observed-vs-normal frost, GDD, garden
  timeline, wildlife firsts) with per-season JSON snapshots so year-over-year self-assembles
  from 2027. Paired with a nightly **house journal** (deterministic day-facts, resident model
  phrases; records event is canonical). Both are [[anti-slop-principle]] all the way down — the
  LLM only ever *phrases*. Field note worth keeping: a year of bird feeding has produced a
  resident garden patrol (5 chickadees, 4 titmice, jays, catbirds, robins) and pest pressure has
  stayed mitigated — logged in hestia's records as this season's working theory, testable
  against pest-window outcomes next year. Ops detail (fixed backup leg, off-site watchdog,
  shopping tool) stays in-repo + hestia's `memory/` per schema.

- **2026-07-03 — ingest: [[funding]] entity page created.** Funding went cross-cutting this
  week (three applications across three different projects), so it earned a page: NLnet $18K
  for the site (pending, decides ~fall), FUTO for hestia ("Let's unplug Alexa", sent today),
  HF/Gradio hackathon for field-station (results 2026-07-10). Fresh research added the
  Arduino×Qualcomm Hackster contest (open to 2026-08-31) and the WILDLABS Awards
  ($10K/$50K, Arm-backed, edge-AI species ID — 2026 missed, EOI ~Jan 2027) as the standout
  [[forager-ml]] match. Principle recorded: one project per application, drafts stay
  private in-repo, the ledger lives here. index.md updated.

- **2026-07-08 — ingest: [[north-star]] page created; roadmap edges filled.** The "lofty goal"
  (wearable/backpack **field brain** — a Pi 5 fusing plant/fungi ID + environmental sensors +
  body sensors + offline maps + mesh comms + on-device judgment) had no wiki home, and the
  "Planned edges" section of [[ligaments]] was empty — that gap is *why* the vision kept feeling
  like it lived only in Alex's head. Distilled from the site repo's `HomesteaderLabsProjectSummary.md`
  (2025-11-01, where the powered backpack was the *original* concept, pivoted to the handheld to
  de-risk), reframed as a **6-rung ladder** (R0 foundation DONE → R5 summit), confirmed by Alex
  2026-07-08. Key reframe on record: the field brain is **two already-built halves** —
  [[forager-ml]] sensing (abstains when unsure) + [[hestia]] judgment (local voice loop) — so the
  summit is *convergence, not invention*. Operating mode named: open-research / data-first, tools =
  intake valve, each rung compounds the data moat. [[index]] + [[ligaments]] planned-edges updated.
  LINT spotted (left for a pass): index.md "Open threads" still lists the pest-alert ligament as
  having no source artifact, but it went LIVE 2026-07-01 per [[ligaments]].

- **2026-07-10 — result: HF/Gradio "build small" hackathon — [[forager-field-station]] did not
  place.** Ledger updated in [[funding]] (entry moved In flight → Resolved); [[index]] one-liner
  and the project page annotated. No winnings, so hestia's NAS/bulk-storage plan stays gated per
  the earmark principle. The submission artifacts all shipped and remain live (Space, published
  weights, demo video, FIELD_NOTES) — the edge-abstention pitch is reusable for the WILDLABS
  (EOI ~Jan 2027) and Arduino×Qualcomm Hackster (to 2026-08-31) entries on the watchlist.
  Winner intel recorded in [[funding]]: the track went to daily-use consumer tools with
  instantly demoable UX (workout tracker 1st; CCTV monitoring; scam-defense) — niche-domain
  depth didn't carry, note for how to pitch the next one.

- **2026-07-26 — pest-alert ligament falsified on the lot; GDD lane picked.** Field observation
  caught a defect no review would have: the 2026 [[hestia]] almanac reported seven aphid windows
  opened across the season while the property saw no aphids at all. Cause was one line in
  `pest_watch._in_window`, which treated a *missing* `gddThreshold` as "open"; aphid rows carry no
  threshold on purpose, because aphids are continuous and multi-generational with no emergence
  event to predict, so they fell through the soil-temp fallback and sat open from spring. A spot
  check of 2025's hot zones found no parasitoid mummies either, so the population never built
  rather than being eaten. Fixed both ends: the site's `pest-companions.json` now carries
  `alertable: false` on the 7 aphid and 2 nematode rows, hestia honours it before either gate
  (regression test added), windows at the reported state drop 20 → 11.
  - **Decision, new page [[gdd-convention]]:** pest thresholds are **base 50 °F accumulated from
    Jan 1**, because we consume published extension thresholds and cannot restate them in another
    frame. This resolves the biofix DIVERGENCE by naming two quantities instead of one: *pest GDD*
    (Jan 1, site) and *season GDD* (observed last frost, hestia's almanac). They read 1608 vs 1500
    at the lot on Jul 25; comparing across them opened windows ~108 GDD late.
  - Sourced and cited there: squash vine borer 900-1000 (genuine inter-source disagreement, publish
    as a window whose width is *derived from local accumulation rate*, not a fixed day count) and
    Japanese beetle 1030 with a 100 °F cutoff — cutoffs are per pest, not global.
  - VERIFY carried forward: the table's existing `100`/`150` thresholds are unsourced and cannot be
    base-50-from-Jan-1; `cabbage-worm` is also inconsistent across crops. Sourcing parked.

- **2026-07-26 (second pass) — pest thresholds sourced; one earlier claim corrected.** Queried five
  extension sources across six pests to test convergence. Three converge cleanly on base 50 from
  Jan 1: squash vine borer, Japanese beetle, imported cabbageworm. Recorded with citations, base,
  biofix and the biological event each number marks, in [[gdd-convention]].
  - **Correction:** the 900-to-1000 vine borer spread was written up as "genuine disagreement
    between sources". It is not. 900 is adult emergence, 1000 is the start of egg-laying, two
    sequential events the sources agree on. The interval is the scouting window, which is more
    useful than a disagreement.
  - **Colorado potato beetle does not fit the convention and should not be forced into it.** Its
    model is 120-200 GDD at base 52 counted from the first adult actually seen. It now carries
    `alertable: false` for a second, distinct reason: not "no emergence event" but "the model needs
    an observation we cannot supply".
  - This explains the figures that looked impossible: the table's `100` and `150` were post-biofix
    numbers compared against a Jan-1 accumulation. Defensible values, wrong frame — the same error
    as the hestia divergence, one layer down.
  - Data changes: cabbageworm 100 → 150 and applied to all three brassicas, resolving the
    cross-crop inconsistency; hornworm's unsourced 150 dropped to soil-temp only; squash vine borer
    added to both squash crops, having been absent despite being the signature squash pest.
