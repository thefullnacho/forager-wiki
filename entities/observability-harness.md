# Observability harness

The batch-replay harness that turns "0.0 toxic-as-edible" from a claim asserted once
into a number verified continuously. Lives in **two** repos at **two grains**, over
**one** shared package.

## The split

| | [[forager-ml]] | [[forager-field-station]] |
|---|---|---|
| Grain | one row per **image** | one row per **K-photo session** |
| Tables | `inference_runs` + `expert_predictions` | `sessions` |
| Key dimension | `source` (hailo / onnx_cpu) | `n_photos` (the two-photo tradeoff) |
| Database | `forager_obs` | `forager_fs_obs` |
| Host port | 5433 | 5434 |
| Compose project | `forager-ml-obs` | `forager-fs-obs` |

**The grains are intentionally different and must not be merged.** forager_ml asks
"per image, did the pipeline get it right, and what did each expert say?" The Space
asks "at K photos, what is the safety and usability tradeoff?" A single schema
would answer neither. This is a divergence by design, not drift.

## `forager-obs` — the shared package

`~/Documents/Forager/forager-obs`, editable-installed as a sibling by both repos'
`observability/requirements.txt`. Holds only what a divergent copy would put at risk:

- **`verdict.py`** — `is_toxic_as_edible`, `is_committed_edible`, `EDIBLE_TIERS`,
  `tier_of`. **The reason the package exists.**
- `db.py` — connect / migrate / `default_dsn`; each repo passes its own SQL dir.
- `writer.py` — `BaseWriter`: the batch `run_id`, commit/rollback.
- `valset.py` — the ImageFolder walk: `group_by_class`, `iter_images`, `k_groups`.

Takes **tiers, not species labels**, so it depends on no metadata table, no model
and no pipeline. Each repo keeps its own label to tier lookup
(`inference/pipeline/safety.py`; `pipeline/metadata.py`). See [[model-registry]].

## Why it was extracted (2026-08-15)

Both repos had independently implemented `toxic_as_edible` — forager_ml as a
property on `InferenceOutcome`, the Space inline in `SessionWriter`. Same rule,
two implementations, and the flagship 0.0 claim resting on both. A safety metric
defined twice can drift once and still read green in both dashboards. The Space's
`convergence.py` had already been hand-ported from forager_ml the same way, which
is the drift this is meant to stop repeating.

## DIVERGENCE: this edge is a hard link, not a vendored snapshot

Every other cross-repo edge in [[ligaments]] is a **vendored** copy plus a
`wikilint vendored-drift` check, per the no-hard-link rule. This one is a real
shared dependency. Deliberate, and the reasoning is narrow:

- The rule exists for **data** (`pest-companions.json`, `frost-zones.json`), where
  a snapshot is legible, diffable, and the consumer must keep working if the
  producer is absent.
- Here the failure mode is the opposite. Vendoring **is what already went wrong**:
  a hand-ported copy of a safety rule that nobody knows to re-check. A drift check
  on executable logic would compare two files that are *supposed* to differ
  (different call sites, different SQL) and could not tell drift from adaptation.
- The coupling is bounded: dev-only, stdlib + psycopg, no runtime consumer.

**The Space runtime never imports it.** `app.py`, `pipeline/` and `game/` are clean;
`deploy.py` excludes `observability/*` from the HF upload (added 2026-08-15 — it had
been shipping the dev harness to the public Space).

## The GitHub repo (RESOLVED 2026-08-15)

`github.com/thefullnacho/forager-obs`, public, default branch `main`. Public
matters: `actions/checkout` uses a token scoped to the repo running the job, so a
private sibling would need a PAT secret in both consumers.

CI in both repos checks it out beside the consuming repo. Verified by anonymous
clone, sibling editable install from a consumer's cwd, and the 36 shared safety
tests running from the cloned copy.

## Not yet shared: the abstention policy

`convergence.py` (single-expert routing, `DEADLY_VETO_FLOOR`,
`EXPERT_CONFIDENCE_THRESHOLD`) is still hand-ported between the two repos — still
true, still not extracted, because it is in the **Space's runtime path** and
extracting it would make the Space depend on a pip install to boot. See
[[model-registry]].

**The different mechanism (2026-08-25): `ops/convergence_drift.py`, a check, not a
shared package.** Not an extraction — a constant-level AST diff between the two
files' `convergence.py`, run as a forager_ml pre-commit hook (local only:
forager-field-station has no GitHub remote, so CI can't check it out the way it
does `forager-obs`). Separates two cases: a named constant present on only one
side is a **DIVERGENCE** (reported, doesn't fail — the two files are legitimately
different implementations, a whole-file diff would be permanently red) versus the
*same* constant holding two different values, which is **DRIFT** and fails. On
first run it found real, live DIVERGENCE that had gone unnoticed: forager_ml's
copy has neither `DEADLY_VETO_FLOOR` nor `EXPERT_CONFIDENCE_THRESHOLD` at all —
only the older `CONFIDENCE_THRESHOLD` / `LOW_CONFIDENCE_THRESHOLD` pair. No DRIFT
(no shared constant disagrees), so nothing failed, but the gap between the two
abstention policies is now visible on every commit instead of living only in this
paragraph.
