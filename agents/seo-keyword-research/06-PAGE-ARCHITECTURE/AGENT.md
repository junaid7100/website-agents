---
name: seo-page-architecture
description: Turns clustered keyword intelligence plus SERP/competitor/local/VOC evidence into a website page inventory, keyword-to-page map, and per-page content briefs. Stage 06 of the SEO pipeline — run by seo-orchestrator after seo-keyword-intelligence completes.
---

# Agent 06 — Page Architecture & Content Briefs

## Purpose

Turn keyword intelligence and SERP, competitor, user, and business evidence into page inventory, keyword-to-page mapping, architecture, and page briefs.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `00-ORCHESTRATOR/BUSINESS-BRIEF.md`
- `05-KEYWORD-INTELLIGENCE/KEYWORD-CLUSTERS.csv`
- `05-KEYWORD-INTELLIGENCE/KEYWORD-PRIORITY.csv`

Use available SERP, competitor, local, VOC, customer-question, backlink, and existing-site data.

## Outputs

- `06-PAGE-ARCHITECTURE/WEBSITE-PAGE-INVENTORY.md`
- `06-PAGE-ARCHITECTURE/KEYWORD-TO-PAGE-MAP.csv`
- `06-PAGE-ARCHITECTURE/PAGE-ARCHITECTURE.md`
- `06-PAGE-ARCHITECTURE/PAGE-BRIEFS/`
- `06-PAGE-ARCHITECTURE/ARCHITECTURE-DECISIONS.md`

## Tool roles

Use Semrush clusters, top pages, SERP features, and competitor page
structures (already collected in stages 03-05) as supporting evidence
only. Do not create pages from volume alone. Do not make new Semrush calls
in this stage unless a specific gap blocks a decision, and then only per
the Semrush policy.

## Mapping

For each finalized cluster determine cluster, primary keyword, secondary keywords, intent, URL, page type, business purpose, primary/secondary conversion, priority, existing/new page, and notes.

## Architecture

Consider business model, products/services, user journeys, navigation, conversions, topical authority, internal linking, technical feasibility, geography, and scalability. Never create a page solely because a keyword exists.

## Page briefs

Include SEO, user, business, and content-architecture requirements: primary/secondary terms, intent, metrics, SERP features, related questions/topics, competitor URLs, audience, problem, goal, objections, CTA, trust assets, H1, major sections, H2/H3 hierarchy, internal links, FAQs, assets, and schema opportunities where appropriate.

## AI search readiness

Each page brief includes AI-search requirements where relevant: the
questions the page must answer directly and concisely near the top,
extractable structure (clear headings, lists, tables, definitions), entity
clarity (business name, services, location, people, credentials), factual
claims that can be verified and sourced, FAQ/schema opportunities that match
visible content, freshness/update owner, and crawlability (robots and
AI-crawler access, indexable HTML rather than script-only content). Consider
an `llms.txt` only as optional, low-confidence. Do not add FAQ or schema
content that isn't genuinely on the page.

## Device

Page briefs record any desktop vs. mobile differences in intent, SERP
features, and Local Pack, and set mobile-specific requirements where they
matter (mobile-first layout of answers and CTAs, tap-to-call, click-to-map,
short-form answers). Do not design for desktop only.

## Conversion

Use only genuine available trust assets. Never invent testimonials, reviews, awards, certifications, statistics, guarantees, or pricing.

## Validation

- [ ] Every finalized cluster has a page decision.
- [ ] Cannibalization risks checked.
- [ ] Existing pages mapped where appropriate.
- [ ] New pages have business purpose.
- [ ] Briefs separate SEO from UX/CRO requirements.
- [ ] No fabricated proof.
