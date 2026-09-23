---
name: seo-measurement
description: Post-launch measurement and optimization loop using Google Search Console and GA4 data against the original keyword-to-page map. Not part of the pre-launch pipeline — run this separately once the site is live and tracking data exists.
---

# Agent 08 — Post-Launch SEO Measurement & Optimization

## Purpose

Create the post-launch measurement loop using Google Search Console and GA4 so keyword research becomes an iterative optimization system.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `06-PAGE-ARCHITECTURE/KEYWORD-TO-PAGE-MAP.csv`
- published URL inventory

Optional: Search Console data, GA4 data, ranking data, business KPI data.

## Outputs

- `08-MEASUREMENT/GSC-QUERY-PAGE-ANALYSIS.csv`
- `08-MEASUREMENT/GA4-LANDING-PAGE-ANALYSIS.csv`
- `08-MEASUREMENT/SEO-OPPORTUNITIES.md`
- `08-MEASUREMENT/CONTENT-REFRESH-QUEUE.csv`
- `08-MEASUREMENT/MEASUREMENT-LOG.md`

## Search Console

Where available analyze queries, clicks, impressions, CTR, average position, page, country, device, and date. Look for high-impression/low-CTR opportunities, unexpected query matches, near-opportunity queries, new demand, cannibalization signals, and geographic/device differences.

## GA4

Where available analyze landing page, organic sessions, engagement, conversions, conversion rate, revenue/value where configured, device, and geography.

## Optimization

Possible actions include title/meta tests, content expansion, internal linking, FAQ additions, consolidation, supporting content, conversion improvements, or technical escalation. Document evidence for every action.

## Validation

- [ ] Data period is explicit.
- [ ] GSC and GA4 metrics are clearly labeled.
- [ ] Visibility data is distinguished from business outcomes.
- [ ] Actions reference observed evidence.
- [ ] Changes are logged.
