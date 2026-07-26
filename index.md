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
- [[dev-box-and-cuda]] — the shared RTX 5080 + 4060 Ti box and the recurring CUDA library-path gotcha.
- [[brand-thesis]] — caloric security / off-grid / "refuses when unsure" + the brand voice.
- [[anti-slop-principle]] — the shared "model on a short leash" stance; hestia's determinism-over-intelligence and forager's abstain-over-bluff are one tenet.
- [[funding]] — the money ledger: NLnet (site, pending) + FUTO (hestia, sent 2026-07-03); HF hackathon (field-station) did not place (2026-07-10); watchlist incl. WILDLABS Jan 2027.
- [[gdd-convention]] — pest thresholds are base 50 °F from Jan 1, full stop; season GDD (from observed last frost) is a different quantity with a different name. Sourced thresholds + citations live here.

## Playbooks (repeatable workflows for any project)
- [[brag-video]] — point `/brag` at any repo → a 15–25s polished launch clip / vertical reel from its own UI.

## Open threads (see `DIVERGENCE:` / `VERIFY:` items)
- DIVERGENCE: field-station serves 3 experts (psychedelics omitted by intent); forager_ml trains 4 — see [[model-registry]].
- VERIFY: field-station serves `domain_router_v2`; forager_ml's published manifest is `domain_router` (v1) — which is canonical? [[model-registry]]
- TODO: the planned [[hestia]] ← pest-alert ligament has no source artifact yet — see [[ligaments]].
- RESOLVED 2026-07-26: the pest-GDD biofix divergence. Lane picked — base 50 °F from Jan 1, because we consume published thresholds and cannot restate them. Hestia's last-frost accumulation stays, renamed as a separate quantity. [[gdd-convention]]
- VERIFY: `pest-companions.json` thresholds (100 colorado-beetle/cabbage-worm, 150 hornworm) are unsourced and cannot be base-50-from-Jan-1 figures; sourcing is parked. [[gdd-convention]]
