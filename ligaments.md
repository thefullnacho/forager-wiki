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
- **[[homesteader-labs-site]] → [[hestia]]** *(frost normals — LIVE 2026-07-01)*: second data
  ligament, same vendoring pattern. The site's `content/frost-zones.json` (NOAA 1991-2020
  frost-date normals by USDA zone, built for `frostNormals.ts`) is consumed by hestia's
  **almanac** (`brain/almanac.py`) to render observed-vs-normal frost lines ("last freeze
  Apr 21 — 47 days later than the zone normal"). The almanac page regenerates nightly and
  snapshots per-season JSON so year-over-year comparisons self-assemble from 2027 on.

## Planned edges (roadmap — not built)
