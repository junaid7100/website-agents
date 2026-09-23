# Website Agents

A library of Claude Code subagents that automate SEO keyword research, copywriting,
and UI/UX design for client website projects — orchestrated by a single head agent.

## Structure

```
agents/                  Specialist subagents, one folder per category
  orchestrator/           The head agent — runs the others in sequence
  seo-keyword-research/
  copywriting/
  ui-ux-design/
.claude/agents            Symlink to agents/, so Claude Code auto-discovers
                           these as usable subagents whenever you work from
                           this repo
```

Client work does **not** live in this repo. Each website/client gets its own
separate project folder (created wherever you like, outside this repo), and
you run the agents from inside that folder. This repo only holds the agent
definitions.

## Agents

| Agent | Folder | Role |
|---|---|---|
| `growth-orchestrator` | `agents/orchestrator/` | Top-level head agent — runs SEO → copywriting → UX/UI in sequence, gated by approval |
| `seo-orchestrator` | `agents/seo-keyword-research/00-ORCHESTRATOR/` | SEO pipeline's own head agent — runs its 8 internal stages |
| `seo-market-seeds` … `seo-copywriting-handoff` | `agents/seo-keyword-research/01..07-*/` | The 7 SEO pipeline stages (dispatched by `seo-orchestrator`, not directly) |
| `seo-measurement` | `agents/seo-keyword-research/08-MEASUREMENT/` | Post-launch SEO measurement loop — dispatched separately once the site is live |
| `copywriting` | `agents/copywriting/` | Phase 2 — turns the SEO handoff into finalized page copy |
| `ux-ui-design` | `agents/ui-ux-design/` | Phase 3 — turns finalized copy + brand assets into a design prompt package |

The SEO stage is itself a mini-pipeline (see
`agents/seo-keyword-research/README.md` and `CLAUDE.md` for its internal
architecture, handoff contract, and Semrush unit-budgeting rules) — its head
agent `seo-orchestrator` is what `growth-orchestrator` actually dispatches
for Phase 1, not each of the 7 inner stages individually.

## Workflow

1. Create a new project folder for the client/website, wherever you keep
   client work.
2. From inside that folder, invoke the `growth-orchestrator` agent. It
   collects one consolidated intake brief, then dispatches `seo-orchestrator`
   (which itself runs the 7-stage SEO pipeline) → `copywriting` →
   `ux-ui-design` in order, stopping for your approval between each phase.
3. Each specialist writes its own output into `0N-<phase>/` inside that
   project folder (the SEO stage further breaks down into its own
   `01-seo-keyword-research/0N-<stage>/` subfolders); the top-level
   orchestrator tracks overall progress in the project folder's `STATUS.md`.

Note: each specialist file carries Claude Code subagent frontmatter
(`name:`/`description:`) so it can be dispatched by name — but subagents are
discovered from the current project's own `.claude/agents/`, not from an
unrelated repo. So **every new client project folder needs this repo's
agents symlinked in** before you invoke the orchestrator there:

```bash
mkdir -p .claude
ln -s /Users/junaid/Documents/website-agents/agents .claude/agents
```

Run that once from inside each new client project folder, then invoke
`growth-orchestrator` from there.

## Adding a new agent

Drop a `.md` subagent file (Claude Code frontmatter format) into the matching
category folder under `agents/`. If it's a new category, create a folder for
it first.
