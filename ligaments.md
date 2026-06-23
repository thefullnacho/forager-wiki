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

## Planned edges (roadmap — not built)

- **[[homesteader-labs-site]] → [[hestia]]** *(pest-alert)*: warn about pest pressure through
  Home Assistant. **Source pinned (2026-06-23):** it's *data, not a model* —
  `content/crops/pest-companions.json` on the site is a **phenology-aware pest-emergence table**:
  per crop, a list of `pests` each with a `soilTempThreshold` (°F) and `gddThreshold`
  (growing-degree-days) for emergence, plus `companions` (trap-crop / repellent interplantings)
  tagged with an `evidenceLevel`. Consumed today by `app/tools/caloric-security/companions/page.tsx`.
  **Why hestia is the natural consumer:** those thresholds are keyed to soil temperature + GDD,
  which hestia already measures per bed (the Ecowitt soil sensors — see hestia's garden memory).
  So the build is: hestia reads `pest-companions.json`, tracks each bed's soil-temp/accumulated
  GDD against the thresholds, and fires an HA alert at the emergence window ("hornworm window
  opening for tomatoes — interplant basil/borage now"). No new ML required; this is a deterministic
  threshold job (HA timers/thresholds, hestia's "determinism over intelligence" north star), not an
  LLM job. This edge makes hestia a full member.
