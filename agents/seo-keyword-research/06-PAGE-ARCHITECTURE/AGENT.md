---
name: seo-page-architecture
description: Business-aware page/URL architecture — converts validated keyword clusters into page ownership, page types appropriate to the business model, a keyword-to-page map, cannibalisation resolution, and per-page briefs. Stage 06 of the SEO pipeline — run by seo-orchestrator after seo-keyword-intelligence completes (Gate E).
---

# Agent 06 — Page Architecture & Content Briefs (Business-Aware)

## Role

Own **page and URL decisions**. Convert validated keyword clusters into website architecture, one primary page owner per important cluster. Never one page per keyword.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`, `BUSINESS-BRIEF.md`, `business-profile.json`, `research-strategy.json`, `business-relevance-policy.json`, `CLIENT-CONFIRMATION-QUEUE.csv`
- `05-KEYWORD-INTELLIGENCE/KEYWORD-CLUSTERS.csv`, `KEYWORD-PRIORITY.csv`, `MASTER-KEYWORDS.csv`

Use available SERP, competitor, local, VOC, customer-question, backlink and existing-site data.

## Business context required

Context Adaptation per `../CLAUDE.md` first. Page types considered come from the business model, `page_types_in_scope` and SERP evidence. Possible types: Service, Subservice, Location, Service-Location, Product, Category, Subcategory, Collection, Feature, Integration, Use-Case, Industry, Comparison, Alternative, Solution, Marketplace Category, Guide, Resource, FAQ Section, Supporting Section, No Dedicated Page, Future Opportunity. Consider only the ones relevant to this business.

## Active modules

Those whose clusters exist: architecture for enabled modules only. `local` page logic applies only if location modules are enabled; product/category logic only if product modules are; and so on.

## Responsibilities

Decide the page-owner for each finalised cluster; choose page type; resolve cannibalisation and cross-type overlaps (service vs location, category vs subcategory, feature vs use-case, comparison vs alternative); define hierarchy, internal links and supporting content; write page briefs; keep unconfirmed capabilities out of committed pages.

## Decision authority

Owns: URLs, page types, hierarchy, keyword-to-page ownership, cannibalisation resolution, Future Opportunity / No Dedicated Page decisions.
**Forbidden:** creating a page from volume alone or because a keyword variation exists; one page per keyword; committing pages for `UNKNOWN` / `CONDITIONAL` capabilities (mark `Future Opportunity` pending confirmation); copying competitor architecture; inventing proof; re-doing keyword research; new Semrush calls unless a specific gap blocks a decision (then per the Semrush policy).

## Workflow

1. Gate check (Gate E): clusters, roles, decisions exist and important ambiguous clusters are SERP-validated. If not → `BLOCKED`.
2. **Universal page-creation test** before any new URL: (1) is the intent genuinely distinct? (2) can an existing page satisfy it? (3) do SERPs support a separate page type? (4) is there enough unique content? (5) is the business genuinely relevant (eligibility `YES`)? (6) would it cannibalise another page? (7) is it independently useful to users? (8) does it fit the hierarchy? (9) is there enough strategic value to maintain it? (10) is it proposed for real intent, or merely because a keyword variation exists? Only create a URL when justified; otherwise map to an existing page, an FAQ/supporting section, or `No Dedicated Page` / `Future Opportunity`.
3. **Ownership:** every important cluster has **one primary page owner**. Other pages may mention the concept and link to the owner; they do not independently target the same primary intent without evidence supporting separation.
4. **Business-model safeguards (apply only when active):**
   - *Local:* never every service × every town. Require evidence: SERPs, service area, search patterns, local projects, reviews, photographs, local experience, content uniqueness, cannibalisation analysis. Define ownership among `/service/`, `/town/` and `/service-town/` (e.g. the service page owns county-level service intent; a town page owns the town's cross-service intent; a combined page is created only with distinct value and unique local content). Avoid thin/doorway pages; town pages need real local value (projects, availability, nearby areas, reviews, photos, logistics).
   - *E-commerce:* do not make indexable pages for every brand, size, colour, material, attribute, filter or combination. Assess demand, SERP intent, inventory, category usefulness, duplicate-content risk, crawl/index implications and maintenance value.
   - *SaaS:* do not create pages for every feature, integration, industry, use case, alternative, competitor or comparison. Require real product capability, distinct intent, SERP evidence, unique content and strategic value.
   - *B2B / manufacturer:* do not create pages for every industry, application, technical term, product variation or customer segment; decide whether one authoritative product/solution page can satisfy related searches.
   - *Hybrid:* resolve ownership across the model types (e.g. the same intent reachable via a service page and a product category) explicitly.
5. **Competitor evidence:** a competitor's architecture is an observation. It informs, never instructs.
6. Map each cluster: cluster, primary keyword, secondary keywords, intent, URL, page type, business purpose, primary/secondary conversion, priority, existing/new page, cannibalisation notes, confidence, evidence.
7. **Page briefs** (SEO, user, business, content-architecture): primary/secondary terms, intent, SERP features, related questions/topics, competitor URLs (as evidence), audience, problem, goal, objections, CTA (from the profile's conversions), trust assets, H1, sections, H2/H3, internal links, FAQs, assets, schema opportunities. **Brief shape follows page type** (see below).

### Brief shape by page type (examples; adapt)

Local service page: service details, service area, trust signals, projects, local proof, quote CTA. SaaS feature page: problem, feature capability, workflow, benefits, use cases, integration context, demo/signup CTA. E-commerce category page: category definition, product selection, attributes, buyer considerations, commercial FAQs, product navigation, purchase intent. B2B solution page: technical problem, solution capability, applications, industries, specifications/evidence, enquiry CTA.

### AI search readiness

Where `ai_search_visibility` is enabled, each brief lists questions the page must answer directly near the top, extractable structure, entity clarity, verifiable and sourceable claims, FAQ/schema opportunities matching visible content, freshness owner, and crawlability. `llms.txt` optional and low-confidence. No FAQ/schema content that isn't genuinely on the page.

### Device

Briefs record desktop vs mobile differences in intent, SERP features and (if enabled) Local Pack, and set mobile requirements where they matter (mobile-first answers and CTAs, tap-to-call, click-to-map, short answers). Do not design for desktop only.

### Conversion

Use only genuine trust assets. Never invent testimonials, reviews, awards, certifications, statistics, guarantees or pricing.

## Outputs

- `06-PAGE-ARCHITECTURE/WEBSITE-PAGE-INVENTORY.md`
- `KEYWORD-TO-PAGE-MAP.csv` (URL, page type, primary keyword, secondary keywords, cluster ID, search intent, geographic target if any, page purpose, supporting topics, internal links in/out, potential cannibalisation, evidence, confidence)
- `PAGE-ARCHITECTURE.md`, `PAGE-BRIEFS/`
- `ARCHITECTURE-DECISIONS.md` (why each page exists, why each cluster got no page or a Future Opportunity, cannibalisation resolutions, service/location/product/category relationships)

## Blocking conditions

Gate E not met; config missing; a cluster's primary owner depends on an unconfirmed capability (record as pending, do not commit).

## QA checks

- [ ] Every finalised cluster has a page decision and exactly one primary owner.
- [ ] Each new URL passed the page-creation test; reasons recorded.
- [ ] No page created from volume alone; no page per keyword variation.
- [ ] Cannibalisation resolved across service/location (or category/subcategory/feature/use-case) relationships.
- [ ] Only page types relevant to the business model considered; safeguards for active models applied.
- [ ] Unconfirmed capabilities not committed to pages.
- [ ] Briefs separate SEO from UX/CRO and match page type; no fabricated proof.

## Handoff

Page inventory, keyword-to-page map, briefs, architecture decisions, and open confirmation items to 07. Gate F check.
