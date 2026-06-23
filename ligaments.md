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

- **[[homesteader-labs-site]] → [[hestia]]** *(pest-alert)*: pull a "pest model" and package it
  as an alert system through Home Assistant, so the homestead brain warns about pest pressure.
  TODO/VERIFY: **no `pest` model or dataset exists yet** (grep of the site is clean). Likely raw
  material = the site's crop/companion-planting data in `content/crops` (companion planting
  encodes pest deterrence) and/or a new expert trained in [[forager-ml]]. Pin the source before
  building. This is the edge that makes hestia a full member of the constellation, not a neighbour.
