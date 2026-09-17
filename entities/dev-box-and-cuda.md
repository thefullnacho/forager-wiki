# Entity: dev box & the CUDA library-path gotcha

The single workstation that both [[forager-ml]] and [[hestia]] run on, and a bug-family that has
now bitten both. Recording it once here saves the next debugging session in either repo.

The coding agents and multiplexer that run on this box live in [[agent-runtime]].

## The box
- **GPUs:** RTX **5080** (Blackwell, `sm_120`, 16 GB) — primary; RTX **4060 Ti** (Ada, `sm_89`,
  16 GB) — secondary. **CPU:** Ryzen 9 9950X3D. ~800 W combined under load (1000 W Gold PSU).
- **GPU allocation discipline (shared convention):** pin work with
  `CUDA_DEVICE_ORDER=PCI_BUS_ID` + `CUDA_VISIBLE_DEVICES`. The 5080 is the resident brain/LLM +
  EfficientNet training; the **4060 Ti** takes overflow — and is *required* for Hailo DFC
  compilation, because TF 2.18 only supports up to `sm_90` (the Blackwell 5080 can't run it).
  Hestia's voice STT also runs on the 4060 Ti, leaving the 5080 for the resident model.

## Display output — fixed 2026-08-13
Until 2026-08-13 the monitor was plugged into the **motherboard**, so X11 rendered the desktop on
the Ryzen **integrated** GPU (`radeonsi`, 512 MB reported) while both NVIDIA cards sat with no
display attached. Any OpenGL application silently got the weakest device in the box; the two
16 GB cards were invisible to it. The cable was moved to the **5080**, confirmed by
`glxinfo -B` reporting `NVIDIA GeForce RTX 5080/PCIe/SSE2` after reboot.

Consequence for the allocation discipline above: `CUDA_VISIBLE_DEVICES` governs *compute* only.
It says nothing about which GPU serves **OpenGL/display**, which is decided by the physical cable
(or by `__NV_PRIME_RENDER_OFFLOAD` + `__NV_PRIME_RENDER_OFFLOAD_PROVIDER`, where `NVIDIA-G0` is
the 5080 and `NVIDIA-G1` the 4060 Ti). A GPU app that looks slow is worth checking with
`glxinfo -B` before profiling anything else.

`VERIFY:` this page records the CPU as a 9950X3D; `lscpu` on 2026-08-13 reported
**"AMD Ryzen 9 9950X 16-Core Processor"** (16C/32T). Worth an eyeball to settle which is right.

## Residency caveat on the resident model
The 5080 hosting a resident LLM is true only *after something loads it*. `OLLAMA_KEEP_ALIVE=-1`
keeps a model pinned once loaded but nothing loads it at boot, so after every reboot the first
request pays the full load cost (measured 5.6 s for `qwen3:14b`, 9.65 GB VRAM). [[hestia]]'s
`/health` reports `ollama: up` either way, so its watchdog cannot see the gap. Tracked in that
repo, not here.

## The gotcha (bites both projects)
Blackwell + a CUDA toolkit means dynamically-loaded CUDA libs aren't always on the default search
path, and **non-interactive contexts (systemd units, subprocess compiles) start with an empty
`LD_LIBRARY_PATH`** while the interactive shell has it set — so code that works in a terminal
fails as a service/job.

- **hestia (2026-06-22):** the Wyoming STT systemd service threw `RuntimeError: Library
  libcublas.so.12 is not found or cannot be loaded` on every transcription until
  `Environment=LD_LIBRARY_PATH=/usr/local/cuda-13.0/lib64` was pinned in the unit.
- **forager_ml:** its README's "Known Issues / CUDA-library path gotcha" is the *same family* —
  the DFC compile must be steered to the 4060 Ti and given the right CUDA lib path.

**Rule of thumb:** if it works in your shell but fails as a service/cron/compile job, suspect
`LD_LIBRARY_PATH` first. Pin the CUDA libdir explicitly in the unit/script.

**2026-09-17, third consumer:** [[homesteader-labs-site]] content work reuses hestia's
faster-whisper (1.2.1, `small.en`, 4060 Ti) via its uv project environment to transcribe post
videos, and ffmpeg on the same box to tonemap iPhone HDR (bt2020 / arib-std-b67) to bt709 before
encode. No new install; the box is now shared by three projects, not two.
