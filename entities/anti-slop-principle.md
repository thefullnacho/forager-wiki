# Entity: the anti-slop principle (model on a short leash)

*(articulated 2026-07-01)*

The constellation's shared engineering stance: **keep the LLM out of anything that has to be
right.** The model does judgment, ranking, and conversation; everything load-bearing —
correctness, schedules, safety — is deterministic scaffolding around it. This is the engineering
face of [[brand-thesis]]'s "abstain over bluff," and the answer to *"isn't this just AI slop / a
chatbox wrapper?"*: the interesting part is deliberately **not** the model.

## The one rule
Point a model at what it's good at (judgment over grounded context) and structurally prevent it
from doing what it's bad at (recall, arithmetic, schedules, unhedged claims). When unsure, the
system **defers or abstains** rather than letting the model bluff. If you ripped this scaffolding
out and let the model freewheel, it *would* be slop — the scaffolding is the project.

## Same tenet, different flavor per repo
- **[[hestia]] — "determinism over intelligence."** Schedules/thresholds/timers → HA + a systemd
  tick; structured state → SQLite records; the model never computes a fire-time or holds a fact
  (its `reminder` tool schema literally says *don't calculate the date yourself, you get it
  wrong*). Answers are **grounded, not recalled** — live catalogs + pinned docs injected into the
  prompt with an anti-fabrication rule. Skill routing is **deterministic keyword-match, not
  model-chosen** — an empirical call (the 14B mis-routes past one option), backed by an eval
  suite; a dense 14B was chosen *over* a larger 30B because the scaffolding beat the bigger model.
  Detail lives in-repo (`memory/`, `ARCHITECTURE.md`) — defer down.
- **[[forager-ml]] / [[forager-field-station]] — "refuses when unsure."** The classifier abstains
  rather than guess a species; the deadly-mushroom veto is safety, not UX. Same instinct: a
  confident-but-wrong answer is the failure mode engineered out. See [[model-registry]].
- **[[homesteader-labs-site]] — the outward framing.** *"A forager in the woods has no signal, so
  a small model that abstains isn't a compromise — it's the only thing that works."* The principle
  doubles as brand ([[brand-thesis]]).

## Why it's here
Cross-cutting and not re-derivable from any one repo: a portfolio-level design value, and the
sharpest defense of the whole constellation against the "just a wrapper" dismissal. A skeptic can
wave off one project as glue code; a *consistent stance about where models belong* across four is
harder to dismiss.
