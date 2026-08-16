# Entity: agent runtime on the shared dev box

How work actually gets done in all four repos. This lives in dotfiles on [[dev-box-and-cuda]],
not inside any repo, so it is not re-derivable from any single codebase — which is why it is
recorded here rather than linked down.

## The agents

Four subscription-backed coding CLIs, no per-token API billing on any of them:

| Agent | Auth | Worktree isolation |
|---|---|---|
| Claude Code | subscription | via its own worktree tooling |
| grok (xAI "Grok Build TUI") | grok.com, i.e. the X Premium sub carries over (confirmed 2026-08-16) | **native `-w/--worktree`** |
| kimi | subscription | **manual only** |
| agy (Antigravity CLI) | AI Pro | no multiplexer integration exists |

`agy` replaced the Gemini CLI, which Google discontinued for individual tiers on 2026-06-18.

## herdr — the multiplexer, added 2026-08-16

`herdr` v0.8.0, Apache 2.0, a tmux-for-coding-agents: persistent panes that survive a dropped
connection, an attention queue of working / blocked / idle across workspaces, git worktree
helpers, and re-attach over SSH or phone.

- **No network surface.** Unix socket only; remote attach rides SSH. There is no bind or port
  setting anywhere in its config, so the "never bind a LAN interface" discipline [[hestia]]
  follows simply does not apply to it.
- **Deliberately no systemd unit.** Its server self-spawns on attach, so a unit would be a second
  supervisor, and the double-supervision failure has already cost this constellation one outage.
- Integrations installed for Claude Code, kimi and grok. They are **asymmetric**: kimi reports 12
  lifecycle events including blocked-on-permission, while Claude Code and grok report session
  start only and their live state is inferred from the pane. Trust the queue accordingly.
- `VERIFY:` pre-1.0 software from a company formed in 2026. Pinned to the `stable` channel.

## The containment rule (unchanged, and now load-bearing)

Headless dispatches auto-approve every tool call. **Git worktree isolation is the actual
blast-radius boundary**; prompt-level "don't touch the repo" is advisory and does not hold.

herdr makes unattended agents easier to run, which raises the stakes rather than lowering them:
its blocked-on-permission signal never fires for headless kimi, because that mode never asks.
**herdr buys visibility, not containment.** grok is the structurally safer headless worker, since
its isolation is a flag on the same command that starts the work rather than a ritual to remember.
