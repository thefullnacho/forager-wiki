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
