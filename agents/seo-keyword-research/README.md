# SEO Keyword Research Agent — Multi-Agent System

## Architecture

```text
00 Orchestrator
      |
      v
01 Market & Seeds
      |
      +----------------+----------------+
      |                |                |
      v                v                v
02 Google        03 Semrush       04 Competitor
Research         Research         & Local SEO
      |                |                |
      +----------------+----------------+
                       |
                       v
              05 Keyword Intelligence
                       |
                       v
              06 Page Architecture
                       |
                       v
              07 Copywriting Handoff
                       |
                       v
              08 Measurement
```

## Fresh-session operation

Every stage can run in a new Claude Code session. The handoff mechanism is the filesystem: manifest, briefs, CSV datasets, analyses, page briefs, and handoff package.

## Suggested execution

`00-ORCHESTRATOR/AGENT.md` (the `seo-orchestrator` agent) walks through
stages 01-07 itself, one at a time, asking permission before each — this is
the normal entry point rather than running each stage by hand:

1. Run `00-ORCHESTRATOR`.
2. Run `01-MARKET-SEEDS`.
3. Run `02-GOOGLE-RESEARCH`, `03-SEMRUSH-RESEARCH`, and `04-COMPETITOR-LOCAL`
   — no dependency between these three, so any order works, but they run
   one at a time (there are no subagent tools to run them concurrently).
4. Run `05-KEYWORD-INTELLIGENCE`.
5. Run `06-PAGE-ARCHITECTURE`.
6. Run `07-COPYWRITING-HANDOFF`.
7. After launch and sufficient data, run `08-MEASUREMENT` separately.

## Re-running

If one stage becomes stale, rerun that stage and only downstream stages whose inputs materially changed.

## Source of truth

Original research files remain authoritative. Handoff folders contain copies for downstream agents.
