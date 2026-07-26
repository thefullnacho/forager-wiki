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

Sourced 2026-07-24, extended and corrected 2026-07-26. All base 50 °F from Jan 1 unless noted.

| Pest | Threshold | Cutoff | Sources agree? |
|---|---|---|---|
| Squash vine borer | **900** adults emerge, **1000** egg-laying begins | not specified | yes, 3 sources |
| Japanese beetle | **1030** emergence starts, 2150 ends | **100 °F** | yes, 2 sources |
| Imported cabbageworm | **150** first adult flight (peak 240, larvae through 630, 2nd flight 830) | not specified | yes |
| Colorado potato beetle | 120-200 GDD **base 52** after an **observed first-adult biofix** | — | different frame, see below |
| Squash bug, tomato hornworm | none publishable | — | no citable extension threshold |

Sources: [Ohio State](https://ohioline.osu.edu/factsheet/ent-0106),
[UMass Amherst](https://ag.umass.edu/vegetable/fact-sheets/squash-vine-borer),
[Illinois Extension](https://extension.illinois.edu/blogs/good-growing/2015-06-16-growing-degree-days-what),
[Iowa State](https://crops.extension.iastate.edu/cropnews/2026/06/japanese-beetles-ahead-schedule-2026),
[USA-NPN](https://www.usanpn.org/data/maps/forecasts/Japanese_beetle),
[UC IPM](https://ipm.ucanr.edu/PHENOLOGY/ma-import_cabbageworm.html),
[Cornell IPM](https://cals.cornell.edu/integrated-pest-management/outreach-education/fact-sheets/colorado-potato-beetle-leptinotarsa-decemlineata-vegetable-ipm-fact-sheet).

**Correction, 2026-07-26.** This page previously called the vine borer's 900-to-1000 spread
"genuine disagreement between sources". It is not. **900 is adult emergence and 1000 is the start
of egg-laying**, two sequential events, and the sources agree on both. That makes the interval more
useful than a disagreement would be: it is the scouting window between the moths arriving and the
damage starting. Still derive its width from the local accumulation rate rather than a fixed number
of days, since the same 100 GDD is 4 days in July and 34 days in April at the same site.

## Some pests cannot use a calendar biofix at all

Colorado potato beetle is the worked example. Its published model counts **120-200 GDD at base 52
from the first adult actually seen**. That is a scouting biofix, and no calendar accumulation can
supply the observation it depends on. Converting it to a Jan-1 figure would mean re-deriving the
phenology, which is the thing this page exists to forbid.

So it carries `alertable: false` with a reason, alongside the continuous pests, for a different
cause: not "no emergence event" but "the model needs an input we do not have".

**This explains the numbers that looked impossible.** The table's original `100` for
colorado-beetle and `150` for cabbage-worm were not wrong figures, they were *post-biofix figures
compared against a Jan-1 accumulation*. Colorado potato beetle's 100 sits inside its real 120-200
band. Same class of error as the hestia divergence, one layer down: defensible numbers, wrong frame.

## What the data now carries

Every published threshold in `pest-companions.json` states its own frame, because a bare number is
unreproducible: `gddThreshold`, `gddBase`, `gddBiofix`, `gddEvent`, and a `source` URL. A test
enforces it (`content/crops/pestCompanions.test.ts`).

Two distinct reasons a pest carries `alertable: false`:

1. **No emergence event.** Aphids are continuous and multi-generational; nematodes are
   soil-resident. Falsified on the lot in 2026, see [[ligaments]].
2. **Model needs an observed biofix.** Colorado potato beetle, above.

## Open

- Squash bug and tomato hornworm have no citable extension threshold. Both fall back to the
  soil-temp gate, which hestia labels lower confidence. Tomato hornworm previously carried an
  unsourced `150`, which is also cabbageworm's first-flight figure and was probably copied.
- Japanese beetle and squash vine borer are the two best-sourced pests we have, but Japanese beetle
  is a generalist and does not map to a single crop row, so it is not yet in the table.
- The feed itself remains parked, see `homesteader-labs-next/docs/PEST_ALERT_FEED_SPEC.md`.
