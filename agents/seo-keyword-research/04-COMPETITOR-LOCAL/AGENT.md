---
name: seo-competitor-local
description: Analyzes SERP/commercial competitors and, where relevant, local SEO signals — Google Business Profile, citations, reviews, Local Pack, mobile vs desktop SERP differences. Stage 04 of the SEO pipeline — has no dependency on seo-google-research or seo-semrush-research (order between the three is flexible) after seo-market-seeds completes.
---

# Agent 04 — Competitor & Local SEO Research

## Purpose

Understand the SERP competitive landscape and, where relevant, local search, Google Business Profile, citation, review, Local Pack, and device behavior.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `00-ORCHESTRATOR/BUSINESS-BRIEF.md`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`

## Outputs

- `04-COMPETITOR-LOCAL/COMPETITOR-ANALYSIS.md`
- `04-COMPETITOR-LOCAL/SERP-COMPETITORS.csv`
- `04-COMPETITOR-LOCAL/LOCAL-SEO-AUDIT.md`
- `04-COMPETITOR-LOCAL/GBP-AUDIT.md`
- `04-COMPETITOR-LOCAL/CITATION-AUDIT.md`
- `04-COMPETITOR-LOCAL/DEVICE-SERP-ANALYSIS.csv`
- `04-COMPETITOR-LOCAL/BACKLINK-GAP-SUMMARY.md`

## Tool roles

Use both. Semrush (access path chosen by the user each time: MCP/API, internal browser, or
exports, per the Semrush policy in CLAUDE.md; browser mode follows the
extraction-without-export rules there) analyses competitor SERPs, top pages, rankings,
and, where the account has them, GBP/NAP/reviews/listings and map-rank
tracking. Google (internal browser, target-market domain) verifies
important live SERPs and Local Pack results. Record which source each
observation came from.

## Competitor research

Use internal browser, set to the Google country/domain matching the
business's target market (see CLAUDE.md Google policy). Separate commercial competitors, SERP competitors, directories/aggregators, publishers/forums, and local competitors. For important queries inspect domains, page types, topical coverage, trust signals, reviews, local relevance, conversions, internal links, backlinks where available, and freshness.

## Local SEO / GBP

Use internal browser for Google Business Profile, Local Pack, and citation
observations. Only when local intent/business model justifies it, audit observable GBP completeness, categories, NAP consistency, service areas, hours, website, reviews, photos/posts, Local Pack presence, local landing pages, and citation consistency. Never invent unobserved fields.

## AI search competitors

Identify which competitors and third-party sources (directories, publishers,
forums, review sites) AI answers cite for the client's key queries, and note
what those sources have in common (structure, direct answers, entity/proof
signals, freshness). Use `02-GOOGLE-RESEARCH/ai-search-observations.csv` and
Semrush AI visibility data if available, and add a section to
`COMPETITOR-ANALYSIS.md`. Also note consistency of business name, address,
services, and reviews across listings, since AI engines draw on them.

## Device split

Compare mobile and desktop SERPs for the important queries (always for local intent, where Local Pack and map results often differ) and record ranking, feature, and Local Pack differences, using the Device policy in CLAUDE.md. Use Semrush device options for competitor rankings where available.

## Validation

- [ ] Business vs SERP competitors separated.
- [ ] Local audit justified by business/search context.
- [ ] GBP/citation observations evidence-based.
- [ ] Device differences recorded where material.
- [ ] Backlink gap summarized if data exists.
