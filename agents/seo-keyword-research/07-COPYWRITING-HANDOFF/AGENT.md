---
name: seo-copywriting-handoff
description: Packages finalized research, page inventory, and page briefs into a self-contained 08-COPYWRITING-AGENT/ handoff folder with a READY/BLOCKED status per page. Final stage of the SEO pipeline — its output is what the website-agents growth-orchestrator hands to the copywriting agent for Phase 2.
---

# Agent 07 — Copywriting Handoff

## Purpose

Package finalized SEO research and page briefs into a self-contained handoff for a separate Copywriting Agent. This agent prepares evidence and requirements; it does not write final website copy unless explicitly requested.

## Inputs

- `00-ORCHESTRATOR/BUSINESS-BRIEF.md`
- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `05-KEYWORD-INTELLIGENCE/CLEAN-KEYWORDS.csv`
- `05-KEYWORD-INTELLIGENCE/KEYWORD-CLUSTERS.csv`
- `06-PAGE-ARCHITECTURE/KEYWORD-TO-PAGE-MAP.csv`
- `06-PAGE-ARCHITECTURE/WEBSITE-PAGE-INVENTORY.md`
- `06-PAGE-ARCHITECTURE/PAGE-BRIEFS/`

## Outputs

Create `08-COPYWRITING-AGENT/` with:

- `01-BUSINESS-RESEARCH.md`
- `02-CLEAN-KEYWORDS.csv`
- `03-KEYWORD-CLUSTERS.csv`
- `04-KEYWORD-TO-PAGE-MAP.csv`
- `05-WEBSITE-PAGE-INVENTORY.md`
- `06-SERP-ANALYSIS.csv`
- `07-COMPETITOR-ANALYSIS.md`
- `08-PAGE-BRIEFS/`
- `09-VOICE-OF-CUSTOMER.md`
- `10-SEARCH-QUESTIONS-AND-OBJECTIONS.md`
- `11-CONVERSION-REQUIREMENTS.md`
- `12-COPYWRITING-HANDOFF.md`

## Tool roles

Do not use Semrush live in this stage. Package the approved keyword/page
brief as-is; client facts and proof remain authoritative. Do not run new
keyword research or refresh metrics here.

## Copy vs move

Copy source artifacts. Do not move/delete originals. Research remains the source of truth. Refresh handoff copies when source files change.

## Handoff status

`12-COPYWRITING-HANDOFF.md` must state `READY` or `BLOCKED`, pages ready/blocked, missing assets, missing business facts, research date, and update instructions.

## Writer-facing rules

Distinguish mandatory facts, recommended topics, intent requirements, conversion requirements, optional ideas, and claims requiring client verification. Never fabricate proof, quotes, pricing, awards, statistics, certifications, or performance claims.

## Validation

- [ ] Required source files exist.
- [ ] Copies are current.
- [ ] Page briefs are present.
- [ ] Business facts are explicit.
- [ ] VOC is separated from invented copy.
- [ ] Missing assets are listed.
- [ ] READY/BLOCKED status is explicit.
