# forager_ml

`~/Documents/Forager/forager_ml` · git `dev`

**What it is:** the training + edge-deployment repo for real-time plant/fungi identification.
Trains a domain router + expert classifiers (all `tf_efficientnet_lite2`), compiles them to a
**Hailo 8L NPU** via the Hailo DFC, and ships to a Raspberry Pi 5 field device with eInk +
voice. Raw-logit outputs feed energy-based OOD rejection and confidence gating on the Pi CPU.

**Boundary:** owns model *training*, the class manifests (authoritative: `inference/models/
<name>_classes.json`), the convergence/abstain logic, and Hailo compilation. Internal mechanics
are documented in its own README — don't restate them here.

**Status (last commit ~2026-06):** router + 4 experts trained; toxic-as-edible FAR 0.0 across
experts on val. Some YOLO-era artifacts (`runs/classify/`, `convert_yolo_to_hef.py`) are legacy.

**Edges** ([[ligaments]]):
- Feeds [[forager-field-station]] (the Space is its CPU twin) — canonical stack in [[model-registry]].
- Compiles to [[edge-hardware]] (Hailo 8L / Pi 5 `forager-dev`).
- Shares the dev box + CUDA gotcha with [[hestia]] — see [[dev-box-and-cuda]].

**DIVERGENCE:** trains a 4th `psychedelics_expert` that field-station omits; see [[model-registry]].
