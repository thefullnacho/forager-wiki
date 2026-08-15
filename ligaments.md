# Ligaments — how the constellation connects

The directed edges between projects. Your mental model: they overlap and **inherit** from one
another. This page is the canonical list of those edges; each project page links back here.

## Live edges (exist today)

- **[[forager-ml]] → [[forager-field-station]]** *(model lineage)*: the Space is the "CPU twin"
  of the on-device stack. Same architecture, same experts — field-station's ONNX models are
  exported from / mirror forager_ml's training output. The canonical stack and where they
  diverge: [[model-registry]].
- **[[forager-ml]] → [[edge-hardware]] → [[homesteader-labs-site]]** *(hardware)*: forager_ml
  compiles to the Hailo 8L and deploys to the Pi 5 handheld; the site *sells* that handheld
  (WALKING MAN PRO). The model and the product are two ends of one object — see [[edge-hardware]].
- **[[forager-ml]] ↔ [[forager-field-station]]** *(shared observability package — LIVE
  2026-08-15)*: both harnesses install `~/Documents/Forager/forager-obs` as an editable sibling.
  It holds the **`toxic_as_edible` rule** plus connect/migrate, the batch-writer contract and the
  ImageFolder val-set walk. The two **schemas stay separate by design** (image-grained vs
  session-grained with `n_photos`) — full detail in [[observability-harness]].
  - **DIVERGENCE (intentional): this is a hard link, not a vendored snapshot.** The one edge that
    breaks the no-hard-link rule. That rule is for *data*, where a snapshot is diffable and
    `wikilint vendored-drift` can check it. Here vendoring is precisely what failed — a
    hand-ported safety rule nobody knows to re-check — and a drift check on executable logic
    cannot tell adaptation from drift. Reasoning in [[observability-harness]].
  - **RESOLVED 2026-08-15:** `thefullnacho/forager-obs` created (public, default branch `main`)
    and pushed. CI in both repos checks it out beside the consuming repo; the path was verified
    by anonymous clone + sibling editable install + the 36 shared safety tests.
- **[[forager-ml]] ↔ [[hestia]]** *(shared dev box)*: both run on the same RTX 5080 + 4060 Ti
  machine; they share GPU-allocation discipline and the same class of CUDA library-path bug —
  see [[dev-box-and-cuda]].
- **[[hestia]] → [[forager-ml]]** *(ops patterns — LIVE 2026-08-15)*: a **pattern** edge, not
  data and not a model. forager_ml's new `ops/` inherits two hestia designs: the
  `snapshot()` + `render()` split from `brain/tools/status.py` (one collector, two consumers —
  here the readout and the watchdog), and the edge-triggered ntfy watchdog from
  `deploy/watchdog/`. Replaces `monitor_jobs.sh`, which tracked jobs by hardcoded PID literals.
  Nothing is copied between the repos: the shape is inherited, the code is written against
  forager_ml's own jobs. **Deliberately not ported:** running off-site. Hestia's watchdog probes
  the house from the dedi because "the house is dark" cannot be reported from inside it;
  forager_ml's jobs are local and the box is already covered by that same probe.
- **[[homesteader-labs-site]] → all** *(brand + voice)*: the thesis (caloric security, off-grid,
  decloudify) and the brand voice (`voice.md`, `newsletter-voice.md`) are inherited whenever any
  project speaks outward — see [[brand-thesis]].
- **[[homesteader-labs-site]] → [[hestia]]** *(pest-alert — LIVE 2026-07-01)*: the edge that made
  hestia a full member. It's *data, not a model*: the site's
  `content/crops/pest-companions.json` (phenology-aware pest-emergence table — per crop, `pests`
  with `soilTempThreshold` °F + `gddThreshold` GDD, and evidence-rated `companions`) is
  **vendored** into hestia (`data/pest-companions.json`, refresh one-liner in
  `brain/pest_watch.py`) per the no-hard-link rule. Hestia accumulates GDD from an *observed*
  biofix (last spring frost found in the Open-Meteo archive), estimates soil temp from trailing
  air temp, and pushes an HA alert once per pest per season when a window opens — companions
  advice included. Fully deterministic (no LLM; the [[anti-slop-principle]] flavor of hestia).
  Spec + limitations: `hestia/brain/PEST_WATCH.md`.
  - VENDORED: `homesteader-labs-site/content/crops/pest-companions.json` -> `hestia/data/pest-companions.json`
  - **2026-07-26, falsified in the field and fixed.** `_in_window` treated a *missing*
    `gddThreshold` as "open" (`... if "gddThreshold" in pest else True`), so pests with no
    threshold fell through to the soil-temp fallback and sat open from spring onward. Every aphid
    entry deliberately has no threshold, because aphids are continuous and multi-generational with
    no emergence event to predict. Result: the 2026 almanac logged **seven aphid windows opened
    across the season while the lot saw no aphids at all**. The table now carries
    `alertable: false` (+ `notAlertableReason`) on the 7 aphid and 2 nematode rows, and hestia
    honours it before either gate. Regression test:
    `brain/tests/test_pest_watch.py::test_alertable_false_never_opens_a_window`.
  - **Biofix divergence, RESOLVED 2026-07-26 → [[gdd-convention]].** The repos were computing two
    different quantities under one name and comparing across them: hestia accumulated from the
    observed last frost (Apr 21, 1500 GDD on Jul 25) while the thresholds assume Jan 1 (1608 GDD,
    same day and place), opening windows ~108 GDD late. Lane picked: **pest thresholds are base 50
    from Jan 1**, since we consume published thresholds and cannot restate them. Hestia's
    last-frost figure survives as *season GDD* for the almanac. Conventions, sourced thresholds
    and citations now live in [[gdd-convention]]; this ligament defers to it.
  - **Code shipped 2026-07-28.** `pest_watch` now carries two accumulators: `pest_gdd` from Jan 1
    (gates alerts) and `cumulative_gdd` from the biofix (season GDD, feeds `almanac.py` and the
    `journal.py` stamp, both unchanged). One archive pass feeds both. Pre-split state is replayed
    from Jan 1 on first load, keeping `alerted` and taking the quiet first-run path, since anything
    it opens today emerged weeks ago. Pinned by `test_pest_gdd_runs_from_jan_1_and_season_gdd_from_the_biofix`
    and `test_alerts_gate_on_pest_gdd_not_season_gdd` in `hestia/brain/tests/test_pest_watch.py`.
    The decision is now enforced rather than only recorded, which is what actually closes it.
- **[[homesteader-labs-site]] → [[hestia]]** *(frost normals — LIVE 2026-07-01)*: second data
  ligament, same vendoring pattern. The site's `content/frost-zones.json` (NOAA 1991-2020
  frost-date normals by USDA zone, built for `frostNormals.ts`) is consumed by hestia's
  **almanac** (`brain/almanac.py`) to render observed-vs-normal frost lines ("last freeze
  Apr 21 — 47 days later than the zone normal"). The almanac page regenerates nightly and
  snapshots per-season JSON so year-over-year comparisons self-assemble from 2027 on.
  - VENDORED: `homesteader-labs-site/content/frost-zones.json` -> `hestia/data/frost-zones.json`

## Planned edges (roadmap — not built)

The future edges that climb toward [[north-star]]. Each maps to a rung; each is independently useful.

- **[[forager-ml]] / [[hestia]] → [[edge-hardware]]** *(convergence, R2)*: merge hestia's memory +
  a light judgment loop onto the Pi 5 handheld so the field device *remembers* sightings (with
  GPS / offline maps), not just classifies. The two halves of the field brain onto one body.
- **[[edge-hardware]] → [[homesteader-labs-site]]** *(data intake, R1)*: opt-in field sightings
  from shipped handhelds flow back as the open-research dataset (forager images + the own-collected
  days-to-maturity moat). The device becomes a consented sensor; the site owns the collection + license.
- **[[edge-hardware]] sensor hub** *(R3)*: low-power BLE environmental sensors (temp / humidity /
  soil / air) fuse with vision on-device — the modular V2 hub.
- **[[edge-hardware]] mesh** *(R4)*: Meshtastic / LoRa (HELTEC already in the site catalog) →
  offline sighting/hazard sharing across a mesh, no cloud.
- **R5 (summit):** body / biometric sensors + powered backpack; the Pi 5 as the engine fusing all
  of the above + on-device judgment. See [[north-star]].
