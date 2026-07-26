# Entity: GDD convention

The canonical statement of how growing degree days are accumulated across
[[homesteader-labs-site]] (computes and publishes) and [[hestia]] (consumes for pest alerts).
Both repos compute GDD; before 2026-07-26 they computed *different quantities* and compared one
against thresholds calibrated for the other. This page fixes the lane.

## The rule

**Pest thresholds are base 50 °F accumulated from January 1.** Any figure compared against a
published pest threshold must use that frame, no exceptions.

Rationale: we do not author pest thresholds, we consume them. Extension services publish them as
base 50 from Jan 1, so that is the only frame in which their numbers mean anything. Restating a
threshold in a different frame would require re-deriving it from the underlying phenology, which
we cannot do and would not be able to cite.

```
GDD_day   = max(0, (Tmax + Tmin) / 2 - 50)      Fahrenheit
GDD_accum = sum of GDD_day from Jan 1 to today
```

Upper cutoffs are **per pest, not global** — Japanese beetle's model specifies 100 °F, others
specify none. Store the cutoff beside the threshold or the number is unreproducible.

## Two quantities, two names

The mistake was one name for two things. Both are legitimate; they are not interchangeable.

| Quantity | Biofix | Used for | Owner |
|---|---|---|---|
| **Pest GDD** | Jan 1 | comparing against published pest thresholds | site `lib/growingDegreeDays.ts` |
| **Season GDD** | observed last frost | crop progress, the almanac's growing-season line | hestia `almanac.py` |

On 2026-07-25 at the lot the two read **1608** (Jan 1) and **1500** (last frost, Apr 21). Comparing
the second against a Jan-1 threshold opens windows ~108 GDD late: 4-5 days in July, weeks in spring
when accumulation runs at 3-9 GDD/day rather than 24.

Hestia keeps its season GDD for the almanac. It must compute pest GDD separately for alerting.

## Sources

Thresholds sourced 2026-07-24, all base 50 °F from Jan 1:

| Pest | Threshold | Cutoff | Source |
|---|---|---|---|
| Squash vine borer | 900-1000 | not specified | [Ohio State Extension](https://ohioline.osu.edu/factsheet/ent-0106), [UMass Amherst](https://ag.umass.edu/vegetable/fact-sheets/squash-vine-borer) |
| Japanese beetle | 1030 (emergence continues to 2150) | 100 °F | [Iowa State Extension](https://crops.extension.iastate.edu/cropnews/2026/06/japanese-beetles-ahead-schedule-2026), [USA-NPN](https://www.usanpn.org/data/maps/forecasts/Japanese_beetle) |

The 900-to-1000 spread on vine borer is genuine disagreement between sources, not a unit mismatch.
Publish it as a window rather than a point, and **derive the window's width from the local
accumulation rate** rather than picking a fixed number of days: the same 100 GDD is 4 days in July
and 34 days in April at the same site.

## Open

- **VERIFY:** the thresholds already in `pest-companions.json` (`100` colorado-beetle and
  cabbage-worm, `150` hornworm) are unsourced and cannot be base-50-from-Jan-1 — the lot stood at
  1608 GDD on Jul 25, so 100 would have been crossed in April. Also inconsistent:
  `cabbage:cabbage-worm` carries 100 while `broccoli:cabbage-worm` and `kale:cabbage-worm` carry
  none. Sourcing is parked; see `homesteader-labs-next/docs/PEST_ALERT_FEED_SPEC.md`.
- Pests with **no emergence event** (aphids, nematodes) carry `alertable: false` and take no
  threshold. A GDD gate cannot predict a continuous population — see [[ligaments]] for how that
  was falsified on the lot.
