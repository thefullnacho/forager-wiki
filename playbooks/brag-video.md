# Playbook: brag video (launch clip / reel)

A repeatable **content lever** for the whole constellation: point the `/brag` Claude Code skill at
any project and it builds a 15–25s polished launch video straight from that repo's code — its real
UI, copy, and palette — then renders to MP4 with music + SFX. Landscape clip or vertical reel.
First run: [[hestia]], 2026-06-23 (a landscape cut + a vertical reel). Crank one whenever there's
something to show off.

## How to run
In Claude Code, inside the target project dir:
- `/brag` — landscape, polished default. Or natural language: `/brag this as a cinematic reel`.
- Flags: `--tone polished|cinematic|app-store|...`, `--format landscape|vertical|square`.
- Output lands in `brag-output/` (or a second dir for variants): `brag-plan.md`, `composition-brief.md`,
  `composition/` (the Hyperframes project), `brag.mp4`, `share-copy.txt`.

## Pipeline (so you can drive / debug it)
1. **Inspect** the repo → 9-question rubric (palette from CSS vars, hero copy, the real UI to show).
2. **brag-plan.md** storyboard — 3–5 scenes, sums to 15–25s.
3. **Hyperframes**: `npx hyperframes init <name> --example warm-grain --resolution landscape|portrait`;
   author a single-file `index.html` with ONE paused GSAP timeline; assets in `assets/music` + `assets/sfx`.
4. **Check**: `npx hyperframes lint && validate && inspect`; `snapshot --at <times>` to eyeball held
   frames *before* rendering (cheap; saves a wasted render).
5. **Render**: `npx hyperframes render --quality high --output ../brag.mp4`.

## Gotchas (learned on the Hestia run — save the next crank)
- **Render deps**: needs system Chrome + ffmpeg (`hyperframes doctor`); both present on
  [[dev-box-and-cuda]]. First `npx hyperframes` install is slow (~7 min — headless-browser deps);
  install once and reuse `node_modules` (symlink it for a second composition).
- **No companion skill bundled**: `/brag` delegates to a `hyperframes` skill that isn't installed.
  The scaffold's `composition/CLAUDE.md` + `npx hyperframes docs <topic>` are the format reference.
- **Fonts**: a Google-Fonts `<link>` FAILS in the sandboxed render (it's a lint *error*). Use
  **`Outfit`** (the `warm-grain` example font — the renderer auto-resolves it); avoid `-apple-system`/
  Segoe/Roboto (not auto-resolved). Generic `monospace`/`sans-serif` keywords are fine.
- **Audio**: every `<audio>` needs an `id` or it renders **silent**. Bundled music is upbeat
  "business-moves" — keep the bed low (`data-volume ~0.3`) for a polished/warm piece.
- **GSAP**: a fade-out ending at a clip-start boundary needs a matching `tl.set(sel,{opacity:0}, t)`
  hard-kill (lint enforces it). **Cap the timeline** to the target length — a trailing
  `tl.to({},{duration},t)` pins it; a yoyo tween can overshoot and leave a black tail.
- **Beat-sync**: bundled tracks ship cue presets (`beats/*.cues.json`). Snap sequential reveals to
  the beat grid; lock 1–3 major moments to strong cues. Hold readable text to its reading floor.

## Existing cuts (clone these as templates)
Two tonal templates — match the tone to the *brand*, not a house style.

**A. [[hestia]] — "quiet premium" (polished).** Warm hearth-orange `#d98a4b` on near-black, `Outfit`
font, soft crossfades, long holds, calm SFX. For an earnest product that speaks for itself.
- Landscape — `~/hestia/brag-output/` (Ask → Answer → All-local → hearth wordmark).
- Vertical reel — `~/hestia/brag-output-reel/` — adds a four-verb beat
  (**Tend · Track · Remember · Enjoy**) → *"the central nervous system for your home."*

**B. [[homesteader-labs-site]] — "field terminal" (cinematic).** Bold `#ff7300` on warm black
`#1c1917`, **monospace** (Courier/terminal), HUD corner-brackets + scanlines, a typewriter hook
(`clip-path: inset` + `steps()` ease), key-clack/click/impact SFX. For a gritty, declarative,
militaristic-homestead brand. Built straight from the site's real tokens + copy
(*"Tools for those who build their own world"* · Hardware/Knowledge/Community · WALKING MAN PRO ·
ACCESS TERMINAL). Output: `~/Documents/Forager/homesteader-site-brag/` (a **sibling dir, NOT inside
the Next.js repo**, so it can't touch the dev server / git). 2026-06-23.
- *Run notes:* reused the installed hyperframes via a `node_modules` symlink (no 7-min reinstall);
  `Caveat` (the site's `font-hand`) is **not** auto-resolved → swapped the handwritten line to a
  mono terminal-comment (`// OFF-GRID · NO CLOUD`). Both confirm the font rule above.

## Cranking it for the other projects
- [[forager-field-station]] — already has a demo video; a brag reel could repurpose the
  abstain / "refuses when unsure" angle.
- [[forager-ml]] — show the router → expert pipeline + deadly-vetoes-safe; palette from its README.
- [[homesteader-labs-site]] — **DONE 2026-06-23** (template B above): field-terminal cut showcasing
  the planting / pest tools + WALKING MAN PRO.

All outward content inherits the thesis + voice in [[brand-thesis]] (off-grid / "no cloud").
