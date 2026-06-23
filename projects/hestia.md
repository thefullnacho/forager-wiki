# hestia

`~/hestia` · git `master`

**What it is:** the homestead brain — an OpenAI-compatible agent (qwen3:14b on Ollama) with
tools (home/media/memory/records/reminder/search/status/weather), a deterministic skills
router, a file-based memory system, and a **voice loop** (Wyoming STT/TTS + a browser mic in the
chat PWA, plus the HA Assist pipeline). Runs as user-systemd services on the GPU box. Deep
internal docs live in-repo (`ARCHITECTURE.md`, `AUDIT.md`, `memory/`) — defer to them.

**Why it's in this wiki (member, not neighbour):** it shares the dev box with [[forager-ml]] and
is the intended consumer of a Forager-trained model (pest alerts via Home Assistant). It's the
homestead's *control + judgment* layer, where the Forager *sensing* layer (plant/pest ID) wants
to surface.

**Boundary:** owns home control, conversation, memory, and the voice plumbing. Its own `memory/`
dir is hestia's *local* wiki — this constellation wiki links to it, never duplicates it.

**Edges** ([[ligaments]]):
- Shares the RTX 5080 + 4060 Ti box and the CUDA library-path gotcha with [[forager-ml]] — see
  [[dev-box-and-cuda]]. (2026-06-22: the `libcublas.so.12` LD_LIBRARY_PATH fix for hestia's
  whisper service is the same bug-family forager_ml flags in its README.)
- **Planned:** consume a pest model from [[homesteader-labs-site]] / [[forager-ml]] and raise HA
  alerts — the ligament that ties sensing to the homestead. VERIFY source; see [[ligaments]].
