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
