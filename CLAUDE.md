# Forager Wiki — schema & maintenance rules

This repo is the **cross-project knowledge layer** for the Forager / Homesteader Labs
constellation. It is an LLM-maintained wiki in the spirit of Karpathy's "LLM wiki": you read
it, the agent writes it, and a schema (this file) keeps the agent a disciplined maintainer
instead of a generic chatbot.

## The three layers

1. **Raw sources** (read, never edited from here): the four project repos themselves —
   `~/hestia`, `~/Documents/Forager/forager_ml`, `~/Documents/Forager/forager-field-station`,
   `~/Documents/Forager/.../homesteader-labs-next-CLEAN`. Their code + per-repo `CLAUDE.md` /
   `README` are the source of truth for *how each repo works internally*.
2. **This wiki** (LLM-written): the connective tissue — what each project IS, how they relate,
   the shared entities they inherit from each other, and the decisions behind divergences.
3. **The schema** (this file): conventions + the ingest/query/lint workflows below.

## The cardinal rule — what belongs here vs. not

Hold only what is **cross-cutting and not cheaply re-derivable from a single repo's code**:
relationships, shared entities, the canonical model registry, brand thesis, and the *why*
behind divergences. **Do NOT** restate file trees, API signatures, or build commands — those
go stale fastest and are owned by each repo's code + `CLAUDE.md`. If a fact lives entirely
inside one repo and that repo's docs already state it, link down to it; don't copy it up.

## Layout

- `index.md` — the catalog: every page with a one-line summary. Keep it current.
- `ligaments.md` — the directed edges between projects (who feeds / inherits / sells / deploys
  whom). This is the heart of the wiki: the constellation overlaps and inherits from itself.
- `projects/*.md` — one page per repo: what it is, its boundary, status, and its edges.
- `entities/*.md` — shared things that live in >1 repo (model registry, hardware, dev box,
  brand). The canonical statement of a shared fact lives here; repos defer to it.
- `log.md` — append-only. Every ingest/decision/lint pass gets a dated line.

## Conventions

- Cross-link liberally with `[[page-name]]` (filename without path/extension). A link to a page
  that doesn't exist yet is fine — it's a TODO marker, not an error.
- Convert relative dates to absolute (YYYY-MM-DD).
- Flag uncertainty explicitly: write `VERIFY:` for things believed-but-unconfirmed, and
  `DIVERGENCE:` for known mismatches between repos (with whether they're intentional).
- Keep pages short. A page that needs scrolling is two pages.

## Operations

- **Ingest** (after meaningful cross-project work): update the affected project/entity pages,
  add/refresh `[[links]]`, update `index.md`, append a `log.md` line. One pass.
- **Query**: answer from the wiki first; if the answer required digging into a repo, file the
  finding back as a page/edit so the next query is cheap.
- **Lint** (periodic health check): hunt `DIVERGENCE:`/`VERIFY:` items, stale claims, orphan
  pages (not linked from `index.md`), and missing back-links. Resolve or re-flag in `log.md`.
