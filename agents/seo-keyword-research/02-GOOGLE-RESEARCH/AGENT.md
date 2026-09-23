---
name: seo-google-research
description: Expands and validates the seed keyword universe using Google Keyword Planner, Trends, autocomplete, PAA, related searches, and manual SERP analysis via the internal browser. Stage 02 of the SEO pipeline — has no dependency on seo-semrush-research or seo-competitor-local (order between the three is flexible) after seo-market-seeds completes.
---

# Agent 02 — Google Research

## Purpose

Use Google-based research to expand, validate, and contextualize the seed universe.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`
- `01-MARKET-SEEDS/CUSTOMER-LANGUAGE.md`

## Outputs

- `02-GOOGLE-RESEARCH/keyword-planner.csv`
- `02-GOOGLE-RESEARCH/trends.csv`
- `02-GOOGLE-RESEARCH/autocomplete.csv`
- `02-GOOGLE-RESEARCH/paa.csv`
- `02-GOOGLE-RESEARCH/related-searches.csv`
- `02-GOOGLE-RESEARCH/serp-observations.csv`
- `02-GOOGLE-RESEARCH/ai-search-observations.csv`
- `02-GOOGLE-RESEARCH/RESEARCH-SUMMARY.md`

## Tool roles

Stay Google-first: Keyword Planner, Trends, Autocomplete, PAA, Related
Searches, and live SERPs for the business's target market. Do not pull
Semrush data in this stage; Semrush may later cross-check these results
(stage 05).

## Google Keyword Planner

Use internal browser, set to the Google country/domain matching the
business's target market (see CLAUDE.md Google policy), not whatever
country/domain the browser opens with by default. Before **every
keyword-entry batch**, verify:

- location
- language
- currency

Use newline- or comma-separated multi-keyword input when supported. Scroll/paginate rows as needed; do not assume the first visible rows are complete.

Capture keyword, average monthly searches, competition indicators, CPC, trend, ideas, source seed, and date where available. Treat volume as search-demand evidence, not guaranteed SEO traffic.

## Google Trends

For important topics inspect 12-month and, where useful, 5-year trends, seasonality, geographic interest, related topics/queries, and rising queries. Do not treat Trends index values as absolute volume.

## Autocomplete

Clear/remove the previous query before entering the next one. Use targeted modifiers first.

A-Z rule:

- Use A-Z expansion only for consumer/informational long-tail discovery where broad discovery is useful.
- Skip A-Z by default for local trades and local B2B.
- State the mode and reason in the research summary.

## PAA and Related Searches

Capture recurring questions, objections, cost, comparison, process, eligibility, location, and troubleshooting queries.

## SERP analysis

For important queries record query, market, location, device, date, top URLs, domains, page types, intent, SERP features, Local Pack, Shopping, Videos, Images, PAA, AI Overview where visible, Featured Snippet, forums, content formats, common topics, conversion elements, trust elements, and freshness.

When material, compare mobile vs desktop SERPs and record differences.

## AI search observation

For important queries (informational, comparison, and high-value commercial),
check whether Google shows an AI Overview (and AI Mode where reachable) in
the target market. Record whether it appears, the answer's main points,
cited source URLs/domains, and whether the client or competitors are cited.
Where the user approves and the engines are reachable, sample the same
queries in ChatGPT search, Perplexity, or Gemini and record the same
fields. Output: `02-GOOGLE-RESEARCH/ai-search-observations.csv`. Follow the
AI search policy in CLAUDE.md.

## Validation

- [ ] Every KWP batch had location/language/currency verified.
- [ ] Result rows were inspected beyond the first screen where needed.
- [ ] Autocomplete query was cleared between tests.
- [ ] Trends seasonality assessed for important topics.
- [ ] Device split captured where material.
- [ ] AI Overview / AI answer observations recorded for important queries.
- [ ] Source/date retained.
