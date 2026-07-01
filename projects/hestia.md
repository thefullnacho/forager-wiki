# hestia

`~/hestia` · **public on GitHub** ([github.com/thefullnacho/hestia](https://github.com/thefullnacho/hestia)), **AGPL-3.0**

> Branch convention (a deliberate divergence): the public remote tracks **`main`** only — a clean
> branch safe to publish; **`master`** is the private full dev history, kept local. Push `main`,
> never `master`/tags. First public push + first Show HN launched 2026-06-28 (see [[log]]).

**What it is:** the homestead brain — an OpenAI-compatible agent (qwen3:14b on Ollama) with
tools (home/media/memory/records/reminder/search/status/weather), a deterministic skills
router, a file-based memory system, and a **voice loop** (Wyoming STT/TTS, a browser mic in the
chat PWA, the HA Assist pipeline, and a **HA Voice PE** kitchen satellite — live 2026-06-29,
full-local: wake word → whisper → hestia → piper, nothing to cloud). Runs as user-systemd
services on the GPU box. Deep
internal docs live in-repo (`ARCHITECTURE.md`, `AUDIT.md`, `memory/`) — defer to them.

**Why it's in this wiki (member, not neighbour):** it shares the dev box with [[forager-ml]] and
is the intended consumer of a Forager-trained model (pest alerts via Home Assistant). It's the
homestead's *control + judgment* layer, where the Forager *sensing* layer (plant/pest ID) wants
to surface.

**Boundary:** owns home control, conversation, memory, and the voice plumbing. Its own `memory/`
dir is hestia's *local* wiki — this constellation wiki links to it, never duplicates it.

**Design stance:** hestia is the constellation's clearest expression of [[anti-slop-principle]] —
"determinism over intelligence": the LLM does judgment + conversation, while schedules, thresholds,
records, and skill-routing are deterministic scaffolding around it.

**Edges** ([[ligaments]]):
- Shares the RTX 5080 + 4060 Ti box and the CUDA library-path gotcha with [[forager-ml]] — see
  [[dev-box-and-cuda]]. (2026-06-22: the `libcublas.so.12` LD_LIBRARY_PATH fix for hestia's
  whisper service is the same bug-family forager_ml flags in its README.)
- **Planned:** consume a pest model from [[homesteader-labs-site]] / [[forager-ml]] and raise HA
  alerts — the ligament that ties sensing to the homestead. VERIFY source; see [[ligaments]].
