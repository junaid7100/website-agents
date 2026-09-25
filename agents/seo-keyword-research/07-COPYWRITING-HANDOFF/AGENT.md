---
name: seo-copywriting-handoff
description: Packages validated page-level keyword strategy and page briefs into a self-contained, page-type-adaptive 08-COPYWRITING-AGENT/ handoff folder with a READY/BLOCKED status per page. Final stage of the SEO pipeline — its output is what the website-agents growth-orchestrator hands to the copywriting agent for Phase 2.
---

# Agent 07 — Content Handoff (Business-Aware)

## Role

Package validated page-level strategy for a separate Copywriting Agent. Prepares evidence and requirements; does not write final website copy unless explicitly requested. Does not restart keyword research.

## Inputs

- `00-ORCHESTRATOR/BUSINESS-BRIEF.md`, `PROJECT-MANIFEST.md`, `business-profile.json`, `research-strategy.json`, `business-relevance-policy.json`, `CLIENT-CONFIRMATION-QUEUE.csv`
- `05-KEYWORD-INTELLIGENCE/CLEAN-KEYWORDS.csv`, `KEYWORD-CLUSTERS.csv`
- `06-PAGE-ARCHITECTURE/KEYWORD-TO-PAGE-MAP.csv`, `WEBSITE-PAGE-INVENTORY.md`, `PAGE-BRIEFS/`

## Business context required

Context Adaptation per `../CLAUDE.md` first. The brief format adapts to each page's type and the business model (see stage 06 for shapes). Client facts and proof stay authoritative.

## Active modules

None run live. This stage consumes the outputs of the modules that were active; it runs no research.

## Responsibilities

Assemble, per page: primary keyword, secondary keyword cluster, search intent, target audience, geographic target (if any), core offering, supporting topics, PAA/questions, relevant entities, competitor/SERP observations, internal links, CTA goal (from the profile's conversions), client facts, claims requiring confirmation, terms to avoid. Flag capability unknowns and open confirmation items per page.

## Decision authority

May: package, copy, mark READY/BLOCKED, list missing facts and assets.
**Forbidden:** new keyword research or metric refresh; changing keyword ownership or URLs; instructing keyword density or exact-match repetition; fabricating proof, quotes, pricing, awards, statistics, certifications or performance claims; treating unconfirmed capabilities as facts; live Semrush use.

## Workflow

1. Verify Gate F: every target page has clear keyword ownership, cannibalisation reviewed, search intent and page purpose documented.
2. Copy source artifacts into `08-COPYWRITING-AGENT/` (copy, never move or delete; research stays the source of truth; refresh copies when sources change).
3. Adapt each page's brief to its page type (local service, SaaS feature, e-commerce category, B2B solution, etc.) and to the profile's audience, conversion and evidence.
4. **Keyword usage rules for writers:** no keyword density; no exact-match repetition. Keywords guide title tag, H1, H2/H3 structure, introduction, offering/product descriptions, FAQs, internal anchor text, image context/alt text where genuinely descriptive, meta description and semantic coverage, in natural language. The page must satisfy the search intent rather than mechanically repeat keywords.
5. Distinguish mandatory facts, recommended topics, intent requirements, conversion requirements, optional ideas, and claims requiring client verification. Pages depending on unconfirmed capabilities are `BLOCKED` (or explicitly conditional) until the queue item is answered.
6. **AI search handoff:** include in `10-SEARCH-QUESTIONS-AND-OBJECTIONS.md` the questions AI answers give for each page's topic, and in each brief the stage-06 AI-search requirements (direct answers, extractable structure, entity facts). Flag claims needing client verification, since AI engines quote page content. Handoff only.
7. **Device handoff:** pass through page-brief device notes (mobile intent, CTAs, answer length). Handoff only.

## Outputs

Create `08-COPYWRITING-AGENT/` with:

- `01-BUSINESS-RESEARCH.md` (includes a plain-language summary of the profile: business type(s), audience, conversions)
- `02-CLEAN-KEYWORDS.csv`, `03-KEYWORD-CLUSTERS.csv`, `04-KEYWORD-TO-PAGE-MAP.csv`
- `05-WEBSITE-PAGE-INVENTORY.md`, `06-SERP-ANALYSIS.csv`, `07-COMPETITOR-ANALYSIS.md`
- `08-PAGE-BRIEFS/` (one brief per page named `<page_id>-<slug>.md`, carried over from stage 06 unchanged)
- `09-VOICE-OF-CUSTOMER.md`, `10-SEARCH-QUESTIONS-AND-OBJECTIONS.md`, `11-CONVERSION-REQUIREMENTS.md`
- `12-COPYWRITING-HANDOFF.md` — states `READY` or `BLOCKED`, pages ready/blocked with reasons (each listed by page key: code + slug), missing assets, missing business facts, open confirmation items, research date, update instructions

## Blocking conditions

Required source files missing; a page has no clear keyword owner or documented intent; a page depends on an unresolved capability confirmation (that page only is `BLOCKED`).

## QA checks

- [ ] Required source files exist; copies current.
- [ ] Every page brief present, page-type-appropriate, with primary keyword, secondary cluster, intent, audience, CTA goal and terms to avoid.
- [ ] Business facts explicit; unsupported claims flagged; VOC separated from invented copy.
- [ ] No keyword-density or exact-match instruction anywhere.
- [ ] Missing assets and open confirmation items listed.
- [ ] READY/BLOCKED status explicit per page.

## Handoff

`08-COPYWRITING-AGENT/` to the Copywriting Agent (Phase 2 of the growth-orchestrator).
