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

## 2026-07-28 — the biofix decision is now enforced, and a linter to keep it that way

- **hestia: pest GDD split from season GDD, shipped.** `pest_watch` carries two base-50
  accumulators: `pest_gdd` from Jan 1 (gates alerts, which is what published thresholds assume)
  and `cumulative_gdd` from the observed biofix (season GDD, unchanged for `almanac.py` and the
  `journal.py` stamp). One archive pass feeds both. Pre-split state replays from Jan 1 on first
  load, keeping `alerted` and taking the quiet first-run path. 97 tests pass; two new ones pin the
  split. `PEST_WATCH.md` updated. See [[ligaments]], [[gdd-convention]].
- **The gap this closed was between the wiki and a repo, not between two repos.** This page and
  [[index]] recorded the divergence as RESOLVED on 2026-07-26 while `pest_watch.py` still carried
  an open `DIVERGENCE:` note, because the lane was picked and the code was not written. Both true,
  about different things, and nothing reconciled them. A prose resolution is a claim about the
  future; only a test makes it a claim about the present.
- **Lint is now executable, not periodic.** Built `wikilint` (in the public `llm-wiki-schema`
  repo): it checks this wiki against the repos it describes and exits non-zero on conflict.
  `unresolved-downstream` found the drift above on its first run. `unpinned-decision` flags any
  canonical page resolved in prose with no test holding it. This is the wiki finally getting its
  own flavour of [[anti-slop-principle]]: the agent proposes, a deterministic check decides.
- **VERIFY:** `dangling-canonical` reports that no repo cites [[anti-slop-principle]],
  [[brand-thesis]], [[dev-box-and-cuda]], [[edge-hardware]], [[model-registry]] or [[funding]].
  Expected for the stance and money pages; worth checking whether [[model-registry]] and
  [[edge-hardware]] should be cited from the repos that implement them.

## 2026-07-28 (later) — vendoring edges are now machine-checked

- **Two `VENDORED:` declarations added to [[ligaments]]**, one per data ligament:
  `content/crops/pest-companions.json` and `content/frost-zones.json`, each site copy paired with
  its hestia consumer. New fourth marker alongside `VERIFY:`, `DIVERGENCE:` and `RESOLVED`.
- **Why it is worth the ceremony.** Both edges are a `cp` plus a promise. If the site edits a
  threshold, hestia keeps serving the old snapshot and nothing says so. `wikilint`'s new
  `vendored-drift` reads both ends and compares them, so no marker is needed in either repo, which
  matters because for this failure nobody knows in advance that there is anything to mark.
- **Verified by breaking it on purpose:** clean, then a one-byte change to
  `hestia/data/frost-zones.json`, which fired with byte counts and the first differing line, then
  reverted byte-identical. Both copies are currently in sync.
- **`wikilint.json` now covers all four repos** including the site. Its absence was the entire
  source of the `broken-path` noise on the first run, which the new `config-error` check would now
  report loudly rather than skipping in silence.

## 2026-08-15 — forager-obs extracted; the first hard-link edge

- **New repo `~/Documents/Forager/forager-obs`**, editable-installed as a sibling by both Forager
  observability harnesses. New page [[observability-harness]]; new edge in [[ligaments]];
  `wikilint.json` now covers five repos.
- **What drove it.** Both repos had their own implementation of `toxic_as_edible` — forager_ml a
  property on `InferenceOutcome`, the Space inline in `SessionWriter` — with the flagship 0.0
  claim resting on both. A safety metric defined twice can drift once and still read green in
  both dashboards. `convergence.py` had already been hand-ported the same way.
- **What was deliberately NOT merged: the schemas.** forager_ml is image-grained
  (`inference_runs` + `expert_predictions`); the Space is session-grained (`sessions`, `n_photos`
  a first-class dimension). They answer different questions. Recorded as intentional so a future
  pass does not "fix" it.
- **DIVERGENCE, intentional:** this is a hard link, not a vendored snapshot — the one exception to
  the no-hard-link rule. That rule is for data, where a snapshot is diffable and `vendored-drift`
  can check it. On executable logic a drift check cannot separate adaptation from drift, and
  vendoring is exactly what failed here.
- **Two latent bugs found by running it rather than reading it.** Both compose files bound host
  port 5433 *and* derived the same Compose project name from their parent directory
  (`observability` in both repos), so bringing up the second one silently RECREATED the first
  repo's container. Now 5433/`forager-ml-obs` and 5434/`forager-fs-obs`, verified running side by
  side for the first time.
- **The Space was shipping its dev harness.** `deploy.py` excluded `scripts/*` but not
  `observability/*`, so the whole harness was being uploaded to the public HF Space. Now excluded.
  Verified the Space runtime never imported it: `app.py`, `pipeline/`, `game/` are clean.
- **Verified end to end**, not just unit-tested: 36 new package tests, 11 forager_ml, 4
  field-station observability, 4 two-photo — all green, against both live databases, with a row
  written through the shared rule and read back out of `v_safety_regression`.
- **VERIFY: `thefullnacho/forager-obs` does not exist on GitHub yet.** CI in both repos checks it
  out beside the consuming repo, so the `observability` workflow fails on any push touching
  `observability/**` until it is created and pushed. Local dev unaffected.
- **Next candidate, not done:** `convergence.py` (single-expert routing, `DEADLY_VETO_FLOOR`,
  `EXPERT_CONFIDENCE_THRESHOLD`) is still hand-ported, but it sits in the Space's *runtime* path,
  so sharing it would make the Space depend on a pip install to boot. Needs a different mechanism.

## 2026-08-15 (later) — hestia's ops patterns ported into forager_ml

- **New `forager_ml/ops/`**: `status.py` (`snapshot()` + `render()`) and `watchdog.py`
  (edge-triggered ntfy). New pattern edge in [[ligaments]]: [[hestia]] → [[forager-ml]], the
  first edge that transfers a *design* rather than data or a model. Nothing copied — the shape
  is inherited, the code is written against forager_ml's own jobs.
- **What it replaced.** `monitor_jobs.sh` tracked jobs by hardcoded PID literals
  (`JOBS=("1271469:medicinals_expert:...")`) written down in one session. Dead at the first
  reboot, the monitor was itself a `while true` process that could die unnoticed, and it exited
  once its listed jobs finished so it never saw the next run. `status.sh`'s dataset-target
  percentages (76000 / 19000) were a fossil too: both downloads finished months ago, so it
  rendered 100% as if it were live progress.
- **Related fossil, NOT fixed:** `retrain_v2.sh` still waits on PIDs 1238285 / 1238290 / 1238583
  via `kill -0`. Those are long dead, so the guard now fails *open* — it skips the wait and
  starts training immediately, which looks like it worked. Flagged, left alone, out of scope.
- **Found by running it, not reading it:** the first collector matched the script name as a
  substring anywhere in a command line, so a *shell* whose argv merely mentioned
  `train_efficientnet_specialist.py` counted as a live training run — as did any grep or editor.
  Now requires an interpreter in argv[0] plus the script as a whole argv token. Pinned by test.
- **Design call worth keeping:** a job that ends with no readable log is reported as "ended,
  unverified", never as finished. Believing a failed overnight run is the expensive mistake.
- 24 tests, stdlib only, no GPU/DB/model. New `ops` workflow — unlike `observability`, it runs
  green today since it needs no Postgres and no forager-obs checkout.

## 2026-08-15 (later still) — forager-obs live; retrain PID fossils removed

- **`thefullnacho/forager-obs` created and pushed** (public, default branch `main`, matching
  forager_ml). Public is load-bearing: `actions/checkout` uses a token scoped to the repo running
  the job, so a private sibling would need a PAT secret in both consumers. The VERIFY in
  [[ligaments]] and [[observability-harness]] is resolved; CI path verified by anonymous clone +
  sibling editable install from a consumer's cwd + the 36 shared safety tests from the clone.
- **Both retrain scripts now wait on running jobs, not PID literals**, via a new
  `ops.status --wait-for` over the same live-command-line detector. Third consumer of that one
  collector, alongside the readout and the watchdog.
- **`retrain_v2.sh` was broken twice over, and had been from the day it was written.** Beyond the
  stale PIDs (which make `kill -0` fail so the guard falls open and training starts on a
  half-downloaded dataset), it used `wait "$pid"` on processes that were never children of its
  shell. `wait` errors instantly on a non-child and `|| true` swallowed it, so that wait never
  waited on any run. `retrain_router.sh` already knew this — its own comment says so and it polls
  `kill -0` instead — which is how the two scripts came to disagree with each other.

## 2026-08-15 (wrap) — project pages caught up

- [[forager-ml]] and [[forager-field-station]] pages now carry the `forager-obs` edge and, for
  forager_ml, the ops-pattern inheritance from [[hestia]]. Both had been updated in
  [[ligaments]] and [[observability-harness]] earlier today but not on their own pages.
- No new edges invented in this pass; only back-links for edges already recorded.

## 2026-08-15 (ingest) — dev box display path + resident-model caveat

- [[dev-box-and-cuda]] gains two facts found while setting up an unrelated game server on the
  same workstation: until 2026-08-13 the monitor was on the **Ryzen iGPU**, not either NVIDIA
  card, so every OpenGL app got a 512 MB device while two 16 GB cards idled. Cable moved to the
  5080. Recorded because `CUDA_VISIBLE_DEVICES` governs compute only and says nothing about the
  display path, which affects any GPU app on this box.
- Same page: the "5080 hosts the resident LLM" claim now carries its caveat. `KEEP_ALIVE=-1`
  pins a model once loaded but nothing loads it at boot, so the first request after a reboot pays
  the load. [[hestia]]'s watchdog is blind to it; tracked in that repo.
- `VERIFY:` flagged a CPU-model mismatch (page says 9950X3D, `lscpu` says 9950X).
- No edges created, changed, or broken. [[ligaments]] untouched.

## 2026-08-16 — ingest: agent runtime on the shared dev box
- New page [[agent-runtime]]: the four subscription-backed coding CLIs (Claude Code, grok, kimi,
  agy) and the `herdr` v0.8.0 multiplexer installed on [[dev-box-and-cuda]] 2026-08-16. Recorded
  here because it lives in dotfiles, not in any repo, so no repo's docs can own it.
- grok CLI auth settled: it signs in with grok.com, so the existing X Premium sub covers it and no
  separate xAI API billing is involved. Models `grok-4.6` (default) and `grok-4.5`.
- The containment rule is restated and sharpened: `herdr` gives visibility, not containment. Its
  blocked-on-permission signal never fires for headless kimi, which auto-approves and never asks.
  Worktree isolation remains the only real boundary; grok is the safer headless worker because
  isolation is a flag rather than a remembered ritual.
- `VERIFY:` herdr is pre-1.0 from a 2026-formed company; pinned to the `stable` channel.
- [[dev-box-and-cuda]] and [[index]] link down to the new page.
- No edges created, changed, or broken. [[ligaments]] untouched.

## 2026-08-20 — ingest: BOM pressure forces a proposed second SKU for edge hardware
- [[edge-hardware]]: Pi 5 handheld BOM risen to ~$450 (rising Raspberry Pi 5 prices), pushing
  retail toward ~$700. Direction under consideration in [[homesteader-labs-site]]: a free/low-cost
  Core ML iPhone app (Apple Silicon Neural Engine, fully on-device inference) as the acquisition
  tier, with the Pi 5 handheld repositioned as the premium "sovereign, no app store" SKU rather
  than the only product.
- App Store guideline research done same-day: 1.4.1 (physical harm / health apps) does not ban
  the category; three comparable apps ship today. The refuse-by-default design already satisfies
  the guideline's actual bar (disclosed methodology, validated accuracy, "consult a professional").
  Separately surfaced a real, documented, category-wide accuracy scandal (Public Citizen: best
  competing app 49% accurate, 44% toxic-species misidentification) — read as market validation of
  [[anti-slop-principle]]'s abstain-over-bluff wedge, not a reason to avoid the category.
  Recommended next step (not started): a minimal TestFlight submission with real safety copy.
- **New planned edge added to [[ligaments]]:** [[forager-ml]] → [[edge-hardware]] (second form
  factor) — a Core ML port of [[model-registry]]'s stack, proposed only, no conversion work
  started. Existing Hailo 8L / Pi 5 edge is unchanged.
- [[index]] open threads carries the pending-decision flag. Status is direction-only: no BOM
  requote, no Core ML port, no TestFlight submission exist yet. Revisit once either side moves.
