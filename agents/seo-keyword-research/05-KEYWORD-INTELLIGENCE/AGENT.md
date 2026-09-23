---
name: seo-keyword-intelligence
description: Cleans, deduplicates, classifies intent, clusters, and prioritizes the combined keyword dataset from all research stages into a master keyword intelligence layer. Stage 05 of the SEO pipeline — run by seo-orchestrator after seo-google-research, seo-semrush-research, and seo-competitor-local all complete.
---

# Agent 05 — Keyword Intelligence

## Purpose

Combine research into a clean, deduplicated, intent-classified, clustered, and prioritized keyword intelligence layer.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`
- Google research outputs
- Semrush outputs if available (metrics and competitor data)
- competitor/local outputs where relevant

## Outputs

- `05-KEYWORD-INTELLIGENCE/MASTER-KEYWORDS.csv`
- `05-KEYWORD-INTELLIGENCE/CLEAN-KEYWORDS.csv`
- `05-KEYWORD-INTELLIGENCE/INTENT.csv`
- `05-KEYWORD-INTELLIGENCE/KEYWORD-CLUSTERS.csv`
- `05-KEYWORD-INTELLIGENCE/KEYWORD-PRIORITY.csv`
- `05-KEYWORD-INTELLIGENCE/INTELLIGENCE-SUMMARY.md`

## Tool roles

Combine Semrush metrics and competitor data with Google evidence. Where
Semrush and Google disagree on volume, intent, or SERP features, record
both and resolve using the live SERP. Semrush may be used here to
cross-check Google-stage results, subject to the Semrush policy.

## Cleaning

Flag/remove wrong geography, wrong language, unrelated meanings, irrelevant audiences/products, jobs, piracy, unrelated brands, duplicates, and out-of-scope queries. Use Keep/Exclude/Review for ambiguity.

## Intent

Classify informational, commercial investigation, transactional, and navigational. Semrush intent is an input; verify important terms against SERPs.

## Clustering

Use topic, intent, semantic relationship, SERP overlap, and whether one page can genuinely satisfy the queries. Do not cluster solely on similar words.

## Primary vs secondary

Each cluster gets one representative primary keyword plus supporting keywords/topics. Do not force exact-match repetition.

## Prioritization

Consider business value, demand evidence, ranking feasibility, SERP strength, intent, topical importance, conversion potential, competitive gap, seasonality, geography, and site strength. Document rationale rather than applying a universal score.

## Validation

- [ ] Duplicates normalized.
- [ ] Wrong geographies removed/flagged.
- [ ] Important intents checked against SERPs.
- [ ] Clusters reflect SERP overlap/topic satisfaction.
- [ ] Primary and secondary terms separated.
- [ ] Priority rationale documented.
