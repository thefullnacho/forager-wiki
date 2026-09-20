# Forager Wiki — index

The cross-project knowledge layer for the **Forager / Homesteader Labs** constellation: four
repos that overlap and inherit from one another. Read [[CLAUDE]] for how this wiki is maintained.
The relationships between projects live in [[ligaments]]; where they're all converging lives in [[north-star]].

## North star (the destination)
- [[north-star]] — the wearable/backpack **field brain** the four repos climb toward, and the 6-rung ladder to it (R0 foundation is DONE; R5 is the powered pack). [[ligaments]] is the present wiring; this is the future one.

## Projects
- [[hestia]] — the homestead brain: HA-integrated agent (LLM + tools + voice) on the GPU box. Member, not neighbour.
- [[forager-ml]] — trains the plant/fungi ID model stack; compiles to Hailo 8L for the Pi 5 field device.
- [[forager-field-station]] — the public HF Gradio Space; CPU twin of the same model stack (hackathon build).
- [[homesteader-labs-site]] — the Next.js brand/content/product site; sells the hardware the model runs on.

## Shared entities (live in >1 repo)
- [[model-registry]] — canonical router+experts stack; the psychedelics & router-version divergences.
- [[edge-hardware]] — Hailo 8L / Pi 5 `forager-dev` / the WALKING MAN PRO handheld.
- [[dev-box-and-cuda]] — the shared RTX 5080 + 4060 Ti box, the recurring CUDA library-path gotcha, and which GPU actually drives the display.
- [[agent-runtime]] — the four coding-agent CLIs on that box, the `herdr` multiplexer added 2026-08-16, and why worktree isolation (not the multiplexer) is the safety boundary.
- [[brand-thesis]] — caloric security / off-grid / "refuses when unsure" + the brand voice.
- [[anti-slop-principle]] — the shared "model on a short leash" stance; hestia's determinism-over-intelligence and forager's abstain-over-bluff are one tenet.
- [[funding]] — the money ledger: **all three 2026 asks closed 2026-09-19.** NLnet (site) declined, FUTO (hestia) never replied in 78 days, HF hackathon (field-station) did not place. Nothing in flight; watchlist incl. WILDLABS Jan 2027.
- [[gdd-convention]] — pest thresholds are base 50 °F from Jan 1, full stop; season GDD (from observed last frost) is a different quantity with a different name. Sourced thresholds + citations live here.
- [[observability-harness]] — the batch-replay harness in both Forager repos: two grains (per image vs per K-photo session) over one shared `forager-obs` package that owns the `toxic_as_edible` rule. The one intentional hard-link edge.

## Playbooks (repeatable workflows for any project)
- [[brag-video]] — point `/brag` at any repo → a 15–25s polished launch clip / vertical reel from its own UI.

## Open threads (see `DIVERGENCE:` / `VERIFY:` items)
- PENDING DECISION (2026-08-20): Pi 5 handheld BOM has risen to ~$450, pushing retail toward
  ~$700. A two-SKU direction (Core ML iPhone app as the acquisition tier, the Pi 5 handheld as
  the premium "sovereign" tier) is under consideration but nothing is built. [[edge-hardware]] [[ligaments]]
- RESOLVED 2026-08-15: `thefullnacho/forager-obs` created (public, default branch `main`) and pushed. The CI path was verified by anonymous clone + sibling editable install + the 36 shared safety tests, so both repos' `observability` workflows go green on the next push. [[observability-harness]]
- NEW 2026-08-25: `convergence.py`'s abstention policy (`DEADLY_VETO_FLOOR`, `EXPERT_CONFIDENCE_THRESHOLD`) still can't be extracted into `forager-obs` (Space runtime path), but now has a check instead — `ops/convergence_drift.py`, a forager_ml pre-commit hook that AST-diffs the two files' named constants and found real DIVERGENCE (forager_ml has neither constant) on first run. Local only, no CI, since forager-field-station has no GitHub remote. [[observability-harness]] [[ligaments]]
- DIVERGENCE (intentional, 2026-08-15): the `forager-obs` edge is a hard link, not a vendored snapshot — the one exception to the no-hard-link rule, because vendoring a safety rule is what caused the drift it fixes. [[observability-harness]] [[ligaments]]
- DIVERGENCE: field-station serves 3 experts (psychedelics omitted by intent); forager_ml trains 4 — see [[model-registry]].
- VERIFY: field-station serves `domain_router_v2`; forager_ml's published manifest is `domain_router` (v1) — which is canonical? [[model-registry]]
- TODO: the planned [[hestia]] ← pest-alert ligament has no source artifact yet — see [[ligaments]].
- RESOLVED 2026-07-26: the pest-GDD biofix divergence. Lane picked — base 50 °F from Jan 1, because we consume published thresholds and cannot restate them. Hestia's last-frost accumulation stays, renamed as a separate quantity. **Code shipped 2026-07-28** (two accumulators, pinned by tests); the decision is enforced, not just recorded. [[gdd-convention]] [[ligaments]]
- RESOLVED 2026-07-26: pest thresholds sourced. Vine borer 900 (emergence) / 1000 (egg-laying), Japanese beetle 1030, cabbageworm 150, all base 50 from Jan 1 with citations. Colorado potato beetle needs an observed biofix so it cannot fire from a calendar; squash bug and hornworm have no citable threshold. [[gdd-convention]]
