# Entity: dev box & the CUDA library-path gotcha

The single workstation that both [[forager-ml]] and [[hestia]] run on, and a bug-family that has
now bitten both. Recording it once here saves the next debugging session in either repo.

## The box
- **GPUs:** RTX **5080** (Blackwell, `sm_120`, 16 GB) — primary; RTX **4060 Ti** (Ada, `sm_89`,
  16 GB) — secondary. **CPU:** Ryzen 9 9950X3D. ~800 W combined under load (1000 W Gold PSU).
- **GPU allocation discipline (shared convention):** pin work with
  `CUDA_DEVICE_ORDER=PCI_BUS_ID` + `CUDA_VISIBLE_DEVICES`. The 5080 is the resident brain/LLM +
  EfficientNet training; the **4060 Ti** takes overflow — and is *required* for Hailo DFC
  compilation, because TF 2.18 only supports up to `sm_90` (the Blackwell 5080 can't run it).
  Hestia's voice STT also runs on the 4060 Ti, leaving the 5080 for the resident model.

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
