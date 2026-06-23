# Entity: model registry

The canonical statement of the plant/fungi ID model stack shared by [[forager-ml]] (trains it)
and [[forager-field-station]] (serves a subset). When the two repos disagree, this page records
which is canonical and *why*. Architecture is shared: `tf_efficientnet_lite2` (timm), 224×224,
raw-logit output (no softmax head) so energy-based OOD rejection + confidence gating run
downstream; resolution is **deadly-vetoes-safe** (a DEADLY flag at/above the veto floor wins).

## The stack

| Model | Domain(s) | Classes | forager_ml (train) | field-station (serve) |
|-------|-----------|--------:|:---:|:---:|
| `domain_router` | berry/mushroom/plant/other | 4 | ✅ `domain_router` (v1 manifest) | ✅ **`domain_router_v2`** |
| `berry_expert` | berry | 11 | ✅ | ✅ |
| `highvalue_expert` | mushroom, plant | 11 | ✅ | ✅ |
| `medicinals_expert` | plant | 21 | ✅ | ✅ |
| `psychedelics_expert` | mushroom | 14 | ✅ | ❌ omitted |

Authoritative class sets = forager_ml's `inference/models/<name>_classes.json` (regenerate from
the checkpoint after any recompile so class order/index stays aligned).

## Divergences

- **DIVERGENCE (intentional — keep):** `psychedelics_expert` is trained in [[forager-ml]] but
  **omitted from the public [[forager-field-station]] Space for optics** (hackathon submission).
  field-station hard-codes 3 experts in `pipeline/infer.py`. Do NOT "reconcile" by adding it to
  the Space. *Open question from the user (2026-06-23):* a belief that the dataset was cleaned so
  forager_ml's structure matches the field-station source-of-truth — **not the case today**:
  forager_ml still carries the 4th expert and a populated `psychedelics_dataset`. The intentional
  split stands; both READMEs should state it so it doesn't read as an accident.
- **VERIFY (likely unintentional):** field-station serves **`domain_router_v2`**; forager_ml's
  published manifest is **`domain_router`** (v1, no `_v2`). Determine which router is canonical
  and align — v2 is probably newer and forager_ml's manifest stale, but confirm.
- **VERIFY (doc drift):** field-station README says experts are `~9M params each`; forager_ml
  says `~4.9M`. Same architecture → one number is wrong. Reconcile the docs.

## Notes
- forager_ml's `convergence.py` `SPECIES_METADATA` must be a *superset* of the deployed manifest;
  it carries an unused `reishi_mushroom` (*G. lucidum*) entry for forward-compat alongside the
  shipped `reishi_northeast` (*G. tsugae*).
