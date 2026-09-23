# Website Agents

A library of instruction files (not Claude Code subagents) that automate SEO
keyword research, copywriting, and UI/UX design for client website
projects — walked through in sequence by a single head agent, in one
continuous conversation, with your explicit permission at every step.

## How this works

There's no Task/Agent tool dispatch here — nothing spawns automatically.
Each "agent" is just a Markdown instruction file. Starting one means Claude
**reads that file and continues the current conversation under its
instructions**; moving to the next agent means reading the next file the
same way. Because everything happens in one continuous session rather than
isolated subagent contexts, **every agent stops and asks for your explicit
permission before every transition** — between phases, between the SEO
pipeline's internal stages, and before drafting each individual copywriting
page. Each of those checkpoints is also a safe point to end the session
(to save tokens, or because context is getting long) and resume later in a
fresh one — every agent checks for its own state file first and picks up
exactly where it left off instead of restarting.

## Structure

```
agents/
  orchestrator/           The head agent — growth-orchestrator
  seo-keyword-research/    A 9-stage mini-pipeline (00-08), with its own
                            head agent (seo-orchestrator) and handoff
                            contract — see its own README.md and CLAUDE.md
  copywriting/
  ui-ux-design/
```

Client work does **not** live in this repo. Each website/client gets its own
separate project folder, and you run the agents from inside that folder.
This repo only holds the agent definitions.

## Agents

| Agent | Folder | Role |
|---|---|---|
| `growth-orchestrator` | `agents/orchestrator/` | Head agent — walks SEO → copywriting → UX/UI in sequence, gated by approval |
| `seo-orchestrator` | `agents/seo-keyword-research/00-ORCHESTRATOR/` | SEO pipeline's own head agent — walks its 8 internal stages, one at a time |
| `seo-market-seeds` … `seo-copywriting-handoff` | `agents/seo-keyword-research/01..07-*/` | The 7 SEO pipeline stages (run by `seo-orchestrator`'s instructions, not directly) |
| `seo-measurement` | `agents/seo-keyword-research/08-MEASUREMENT/` | Post-launch SEO measurement loop — run separately once the site is live |
| `copywriting` | `agents/copywriting/` | Phase 2 — turns the SEO handoff into finalized page copy, one page at a time |
| `ux-ui-design` | `agents/ui-ux-design/` | Phase 3 — turns finalized copy + brand assets into a design prompt package |

The `name:`/`description:` frontmatter on each file is identifying metadata
only — it's not required for this workflow, since nothing auto-discovers or
dispatches these by name. It's there in case you ever want to wire actual
Task/Agent tool dispatch back in for a given agent.

## Workflow

1. Create a new project folder for the client/website, wherever you keep
   client work.
2. Copy (or symlink) this repo's `agents/` folder into that project folder,
   e.g. `cp -R /path/to/website-agents/agents <project-folder>/agents`.
   Copying freezes that project's copy — future fixes made in this repo
   won't reach it unless you re-copy. Symlinking instead keeps it in sync
   automatically.
3. From inside that project folder, tell Claude to read
   `agents/orchestrator/orchestrator.md` and follow it, e.g.: *"Read
   agents/orchestrator/orchestrator.md and use it to start this website
   project."*
4. It collects one consolidated intake brief, then — after your explicit
   permission at every step — walks through the SEO pipeline, copywriting
   (page by page), and UX/UI design in order.
5. Each phase writes its own output into `0N-<phase>/` inside that project
   folder (the SEO stage further breaks down into its own
   `01-seo-keyword-research/0N-<stage>/` subfolders); the head agent tracks
   overall progress in the project folder's `STATUS.md`.
6. If you stop mid-project (new session, saving tokens, or just pausing),
   just repeat step 3 later — every agent checks its own state file first
   and resumes instead of restarting.

## Adding a new agent

Drop a `.md` instruction file into the matching category folder under
`agents/`. If it's a new category, create a folder for it first. Give it
`name:`/`description:` frontmatter for identification, and make sure
whichever agent should hand off to it names the file explicitly (mirror how
`growth-orchestrator` names `seo-orchestrator`'s file path, for example).
