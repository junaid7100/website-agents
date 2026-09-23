---
name: seo-measurement
description: Post-launch measurement and optimization loop using Google Search Console, GA4, GBP, and Semrush data against the original keyword-to-page map. Not part of the pre-launch pipeline — run this separately once the site is live and tracking data exists.
---

# Agent 08 — Post-Launch SEO Measurement & Optimization

## Purpose

Create the post-launch measurement loop using Google Search Console and GA4 so keyword research becomes an iterative optimization system.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `06-PAGE-ARCHITECTURE/KEYWORD-TO-PAGE-MAP.csv`
- published URL inventory

Optional: Search Console data, GA4 data, Google Business Profile data, Semrush ranking/backlink/site-health data, business KPI data.

## Outputs

- `08-MEASUREMENT/GSC-QUERY-PAGE-ANALYSIS.csv`
- `08-MEASUREMENT/GA4-LANDING-PAGE-ANALYSIS.csv`
- `08-MEASUREMENT/SEO-OPPORTUNITIES.md`
- `08-MEASUREMENT/CONTENT-REFRESH-QUEUE.csv`
- `08-MEASUREMENT/AI-SEARCH-LOG.csv`
- `08-MEASUREMENT/MEASUREMENT-LOG.md`

## Tool roles

- **Semrush** (access path asked each time: MCP/API, internal browser, or exports, per the Semrush policy; browser mode follows the extraction-without-export rules): rankings/position tracking, backlinks, site health, and competitor monitoring.
- **Google Search Console, GA4, and Google Business Profile**: first-party performance and enquiries. These are authoritative for clicks, sessions, conversions, calls, and direction requests.

Label every metric with its source; Semrush ranking estimates are not a substitute for GSC data.

## Search Console

Where available analyze queries, clicks, impressions, CTR, average position, page, country, device, and date. Look for high-impression/low-CTR opportunities, unexpected query matches, near-opportunity queries, new demand, cannibalization signals, and geographic/device differences.

## GA4

Where available analyze landing page, organic sessions, engagement, conversions, conversion rate, revenue/value where configured, device, and geography.

## AI search visibility

Re-sample the tracked queries in Google AI Overviews and, where approved,
other answer engines on a set cadence, using the same fields as
`02-GOOGLE-RESEARCH/ai-search-observations.csv`, plus Semrush AI visibility
where available. In GA4 (and referrer data), look for traffic from AI
engines (e.g. chatgpt.com, perplexity.ai, gemini.google.com, copilot) and
report it separately from organic search. Log to
`08-MEASUREMENT/AI-SEARCH-LOG.csv`. Treat results as samples, not rankings.

## Optimization

Possible actions include title/meta tests, content expansion, internal linking, FAQ additions, consolidation, supporting content, conversion improvements, or technical escalation. Document evidence for every action.

## Validation

- [ ] Data period is explicit.
- [ ] GSC, GA4, GBP, and Semrush metrics are clearly labeled by source.
- [ ] Visibility data is distinguished from business outcomes.
- [ ] Actions reference observed evidence.
- [ ] Changes are logged.
