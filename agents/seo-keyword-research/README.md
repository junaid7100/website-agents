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

1. Run `00-ORCHESTRATOR`.
2. Run `01-MARKET-SEEDS`.
3. Run `02-GOOGLE-RESEARCH`, `03-SEMRUSH-RESEARCH`, and `04-COMPETITOR-LOCAL` independently.
4. Run `05-KEYWORD-INTELLIGENCE`.
5. Run `06-PAGE-ARCHITECTURE`.
6. Run `07-COPYWRITING-HANDOFF`.
7. After launch and sufficient data, run `08-MEASUREMENT`.

## Re-running

If one stage becomes stale, rerun that stage and only downstream stages whose inputs materially changed.

## Source of truth

Original research files remain authoritative. Handoff folders contain copies for downstream agents.
