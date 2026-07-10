# North star — the field brain

Where the constellation is converging. [[ligaments]] is how the four repos wire together *today*;
this is the *destination* they climb toward, and the ladder to it. Confirmed with Alex 2026-07-08;
distilled from the site repo's `HomesteaderLabsProjectSummary.md` (2025-11-01).

## The destination
A **wearable / backpack field brain**: a Pi 5 as the engine of a powered pack that fuses, fully
offline — plant/fungi **ID** + **environmental sensors** + **body/biometric sensors** + **offline
maps** + **mesh comms** + on-device **judgment**. This was the *original* 2025 concept; the
handheld ([[edge-hardware]]) was a deliberate de-risking pivot, not an abandonment. Sequenced, not
shrunk.

## Why it's reachable — the two halves already exist
The field brain is two systems, both already built and working, just *separately*:
- **Sensing** — [[forager-ml]] on [[edge-hardware]]: sees, and *abstains when unsure*.
- **Judgment** — [[hestia]]: agent + memory + a fully-local voice loop.

So the summit is **convergence** (merge them onto one body; add sensors, maps, mesh), not
invention. This is the standing answer to "I can't see how I get there."

## The ladder — each rung independently useful, each feeds the data moat
- **R0 — Foundation (DONE):** sensing that abstains, public proof ([[forager-field-station]]),
  home brain ([[hestia]]), funnel + KB + data ([[homesteader-labs-site]]), live site→hestia
  ligaments. *We are standing on this.*
- **R1 — Ship the handheld** as a rugged offline product → revenue anchor **and** the consented
  **data-intake valve** (opt-in field sightings → the open dataset).
- **R2 — Memory + place:** hestia-lite memory + GPS / offline maps on-device → spatially-grounded
  sightings, not just classifications.
- **R3 — Sensor hub:** low-power BLE environmental sensors fuse with vision → "edge sensor
  distillation"; a field-sensing platform, not just a forager.
- **R4 — Mesh:** Meshtastic / LoRa (HELTEC already in the catalog) → offline sighting/hazard
  sharing across a mesh of our own units, no cloud.
- **R5 — Summit:** body/biometric sensors + powered backpack; the Pi 5 fuses ID + environment +
  body + maps + mesh + judgment. The original concept, reached by convergence.

## Operating mode — how we climb
**Open-research / data-first.** The tools are the **intake valve** — free by design; don't try to
monetize them. The durable value is **data + trust + hardware + the local-first stack**, and every
rung compounds the dataset. Monetize the *outputs* (hardware now, grants mid-term, data/models
long-term), never the intake. Per-rung funder fit is tracked in [[funding]].
