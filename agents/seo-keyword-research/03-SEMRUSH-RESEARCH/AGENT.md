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
- `03-SEMRUSH-RESEARCH/ai-visibility.csv` (where available)
- `03-SEMRUSH-RESEARCH/SEMRUSH-USAGE-LOG.md`
- `03-SEMRUSH-RESEARCH/RESEARCH-SUMMARY.md`

## Tool roles

Semrush is the **primary** tool for this stage: Keyword Magic, Keyword
Overview, Keyword Gap, Organic Research, Top Pages, SERP features, and
Backlink Gap. Google research is supporting context only. Log any
Keyword Overview / SERP-feature data alongside the Keyword Magic output
(add columns or a note in `RESEARCH-SUMMARY.md`).

## Browser extraction (no export)

If the user chooses the internal browser, follow "Semrush browser
extraction without export" in CLAUDE.md: apply filters first so only the
rows this stage needs are shown, read that table on screen, write CSV rows
directly into `03-SEMRUSH-RESEARCH/`, save per page, stop when rows stop
being relevant, verify against "Total results", and checkpoint with the user
after each seed/report. If a report can't be read or a plan cap blocks it,
ask the user to supply that report as a file in this folder; never request
access to Downloads.

## Keyword Magic Tool

Use correct country/database. Preserve keyword, volume, KD, intent, CPC, SERP features, trend, and other decision-relevant fields. Do not impose universal KD/volume thresholds.

## Keyword Gap

Compare client and approximately 3–5 strong competitors where appropriate. Capture shared, missing, weak, competitor-only keywords and ranking URLs where available.

## Organic Research / Top Pages

For major competitors capture keyword, position, volume, KD, intent, URL, traffic estimate where available, SERP features, and domain. For top pages capture URL, page type, keyword/topic cluster, ranking terms, and business role.

## Backlink gap

Where available, compare referring domains and target pages. Record competitor(s), target page, link context/type where available, relevance, replicability, and notes. Treat this as opportunity evidence, not a guaranteed outreach list.

## AI search visibility

Where the account and chosen access path expose them, capture Semrush AI
Overview/AI visibility data: keywords triggering AI Overviews, which
domains are cited, and client vs competitor AI visibility. Save to
`03-SEMRUSH-RESEARCH/ai-visibility.csv`. If the account lacks these reports,
record that after a real check; do not estimate.

## API unit budgeting

Estimate units before expensive calls; reuse data; avoid redundant calls; log estimated/actual units and balance; for browser mode log filters, sort, pages read, rows captured vs. "Total results", and any plan cap hit; stop when insufficient; ask before material budget consumption; never invent unit costs.

## Validation

- [ ] Semrush mode explicitly selected.
- [ ] Correct database/country used.
- [ ] Required data returned.
- [ ] Unit usage logged if exposed.
- [ ] Browser mode: filters, pages read, captured vs. total rows, and caps logged.
- [ ] No fabricated metrics.
- [ ] Backlink gap captured where available.
