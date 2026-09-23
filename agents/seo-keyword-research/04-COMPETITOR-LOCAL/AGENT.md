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

## Competitor research

Separate commercial competitors, SERP competitors, directories/aggregators, publishers/forums, and local competitors. For important queries inspect domains, page types, topical coverage, trust signals, reviews, local relevance, conversions, internal links, backlinks where available, and freshness.

## Local SEO / GBP

Only when local intent/business model justifies it, audit observable GBP completeness, categories, NAP consistency, service areas, hours, website, reviews, photos/posts, Local Pack presence, local landing pages, and citation consistency. Never invent unobserved fields.

## Device split

When material, compare mobile and desktop SERPs for the same query and record ranking/feature/local-pack differences.

## Validation

- [ ] Business vs SERP competitors separated.
- [ ] Local audit justified by business/search context.
- [ ] GBP/citation observations evidence-based.
- [ ] Device differences recorded where material.
- [ ] Backlink gap summarized if data exists.
