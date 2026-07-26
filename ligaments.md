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
- **[[forager-ml]] ↔ [[hestia]]** *(shared dev box)*: both run on the same RTX 5080 + 4060 Ti
  machine; they share GPU-allocation discipline and the same class of CUDA library-path bug —
  see [[dev-box-and-cuda]].
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
  - **2026-07-26, falsified in the field and fixed.** `_in_window` treated a *missing*
    `gddThreshold` as "open" (`... if "gddThreshold" in pest else True`), so pests with no
    threshold fell through to the soil-temp fallback and sat open from spring onward. Every aphid
    entry deliberately has no threshold, because aphids are continuous and multi-generational with
    no emergence event to predict. Result: the 2026 almanac logged **seven aphid windows opened
    across the season while the lot saw no aphids at all**. The table now carries
    `alertable: false` (+ `notAlertableReason`) on the 7 aphid and 2 nematode rows, and hestia
    honours it before either gate. Regression test:
    `brain/tests/test_pest_watch.py::test_alertable_false_never_opens_a_window`.
  - **DIVERGENCE (open): the two repos use different biofixes.** Hestia accumulates GDD from the
    observed last-frost biofix (2026: Apr 21, reading 1500 GDD on Jul 25). The published extension
    thresholds the table is *meant* to hold are base 50 **accumulated from Jan 1** (squash vine
    borer 900-1000, Japanese beetle 1030 with a 100 °F cutoff); the site's
    `lib/growingDegreeDays.ts` reads 1608 for the same day and place. Comparing a last-frost
    accumulation against a Jan-1 threshold opens windows **late** by ~108 GDD, which is 4-5 days
    in July but weeks in spring. Pick one convention before the thresholds are sourced.
  - **VERIFY: the thresholds in the table are unsourced.** `100` for colorado-beetle and
    cabbage-worm and `150` for hornworm cannot be base-50-from-Jan-1 figures; the lot stood at
    1608 GDD on Jul 25, so a threshold of 100 would have been crossed in April and every pest
    would read active all summer. Also inconsistent: `cabbage:cabbage-worm` carries 100 while
    `broccoli:cabbage-worm` and `kale:cabbage-worm` carry none. Sourcing is parked — see
    `homesteader-labs-next/docs/PEST_ALERT_FEED_SPEC.md`.
- **[[homesteader-labs-site]] → [[hestia]]** *(frost normals — LIVE 2026-07-01)*: second data
  ligament, same vendoring pattern. The site's `content/frost-zones.json` (NOAA 1991-2020
  frost-date normals by USDA zone, built for `frostNormals.ts`) is consumed by hestia's
  **almanac** (`brain/almanac.py`) to render observed-vs-normal frost lines ("last freeze
  Apr 21 — 47 days later than the zone normal"). The almanac page regenerates nightly and
  snapshots per-season JSON so year-over-year comparisons self-assemble from 2027 on.

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
