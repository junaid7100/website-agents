---
name: seo-semrush-research
description: Collects paid Semrush SEO data — keyword expansion, competitor keyword portfolios, keyword gaps, top pages, backlink gaps — via MCP/API, the internal browser, or manual export, with API unit budgeting. Stage 03 of the SEO pipeline — has no dependency on seo-google-research or seo-competitor-local (order between the three is flexible) after seo-market-seeds completes.
---

# Agent 03 — Semrush Research

## Purpose

Collect paid SEO data for keyword expansion, competitor portfolios, ranking URLs, keyword gaps, top pages, and backlink-gap research.

## First action

Ask:


> How should I access Semrush this time: (1) MCP/API, (2) the internal browser (you stay logged in to Semrush there), or (3) manual steps where you bring the data back?

Ask this every time, including on re-runs; do not assume access or reuse a previous choice.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`

Optional: competitor domains and Google research.

## Outputs

- `03-SEMRUSH-RESEARCH/keyword-magic.csv`
- `03-SEMRUSH-RESEARCH/keyword-gap.csv`
- `03-SEMRUSH-RESEARCH/organic-research.csv`
- `03-SEMRUSH-RESEARCH/top-pages.csv`
- `03-SEMRUSH-RESEARCH/backlink-gap.csv`
- `03-SEMRUSH-RESEARCH/SEMRUSH-USAGE-LOG.md`
- `03-SEMRUSH-RESEARCH/RESEARCH-SUMMARY.md`

## Tool roles

Semrush is the **primary** tool for this stage: Keyword Magic, Keyword
Overview, Keyword Gap, Organic Research, Top Pages, SERP features, and
Backlink Gap. Google research is supporting context only. Log any
Keyword Overview / SERP-feature data alongside the Keyword Magic output
(add columns or a note in `RESEARCH-SUMMARY.md`).

## Keyword Magic Tool

Use correct country/database. Preserve keyword, volume, KD, intent, CPC, SERP features, trend, and other decision-relevant fields. Do not impose universal KD/volume thresholds.

## Keyword Gap

Compare client and approximately 3–5 strong competitors where appropriate. Capture shared, missing, weak, competitor-only keywords and ranking URLs where available.

## Organic Research / Top Pages

For major competitors capture keyword, position, volume, KD, intent, URL, traffic estimate where available, SERP features, and domain. For top pages capture URL, page type, keyword/topic cluster, ranking terms, and business role.

## Backlink gap

Where available, compare referring domains and target pages. Record competitor(s), target page, link context/type where available, relevance, replicability, and notes. Treat this as opportunity evidence, not a guaranteed outreach list.

## API unit budgeting

Estimate units before expensive calls; reuse data; avoid redundant calls; log estimated/actual units and balance; stop when insufficient; ask before material budget consumption; never invent unit costs.

## Validation

- [ ] Semrush mode explicitly selected.
- [ ] Correct database/country used.
- [ ] Required data returned.
- [ ] Unit usage logged if exposed.
- [ ] No fabricated metrics.
- [ ] Backlink gap captured where available.
