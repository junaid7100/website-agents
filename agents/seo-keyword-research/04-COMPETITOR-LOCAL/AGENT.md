---
name: seo-competitor-local
description: Adaptive competitor and market evidence — business, SERP, and (where relevant) Local Pack, content, product, category, solution, or marketplace competitors; local SEO signals only when the business needs them. Stage 04 of the SEO pipeline — has no dependency on seo-google-research or seo-semrush-research (order between the three is flexible) after seo-market-seeds completes.
---

# Agent 04 — Competitor / Market Research (Adaptive)

## Role

Understand the competitive landscape that matters for *this* business and pass observations, not instructions, to Agents 05 and 06. Local SEO work runs only when the strategy enables it.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`, `BUSINESS-BRIEF.md`, `business-profile.json`, `research-strategy.json`, `business-relevance-policy.json`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`
- Optionally 02/03 outputs (organic-competitor lists, SERP observations)

## Business context required

Context Adaptation per `../CLAUDE.md` first. Competitor types come from `competitor_types_active`:

| Type | Typically primary when |
|---|---|
| Business competitors | always (user-supplied) |
| SERP competitors | always (who actually ranks) |
| Local Pack competitors | local intent + physical presence or service area |
| Content competitors | publishers, SaaS, informational-heavy strategies |
| Product / Category competitors | product or catalogue businesses |
| Solution competitors | SaaS / B2B problem-solution search |
| Marketplace competitors | marketplaces, catalogue categories |

Only activate the relevant categories. Business, SERP and Local Pack competitors are frequently different businesses; keep them separate.

## Active modules

`serp_analysis`, `competitor_keyword_research` (with 03), `content_gap_research`, `local_pack_analysis` + `google_business_profile_analysis` (local only), `mobile_desktop_serp_comparison`, `ai_search_visibility`, `backlink_gap_research` (summary depth per strategy).

## Responsibilities

For representative high-value queries inspect the SERP and the competitors relevant to the active types: ranking page types, directories/aggregators/marketplaces, service/product/category/solution/comparison pages, review signals, terminology, structure, location targeting (if enabled), and, if local modules are enabled, Local Pack, GBP categories, citations. Record which source (Semrush vs live Google) each observation came from.

## Decision authority

May: classify competitor types, record page-type and structure observations, propose candidate terms/topics from competitor evidence with eligibility and provisional decision.
**Forbidden:** copying competitor architecture or strategy as instruction (a competitor's `/service-town/` pages, `/competitor-alternative/` pages, or thousands of indexed filters prove nothing about what the client needs); treating a competitor ranking as proof of relevance; concluding what pages the client should have (Agents 05/06); running the local audit for a business where local modules are disabled.

## Workflow

### Tool roles

Use both. Semrush (access path chosen by the user each time, per the Semrush policy; browser mode follows the extraction-without-export rules) analyses competitor SERPs, top pages, rankings and, where the account has them, GBP/NAP/reviews/listings and map-rank tracking. Google (internal browser, target-market domain) verifies important live SERPs and, where enabled, Local Pack.

### Google domain and market

Use the target-market Google domain (and city/postcode where local modules are enabled) for all Google, Maps and Local Pack observations; verify the market from the results; record domain, country, location. See the Google policy in CLAUDE.md.

### Competitor research

Use the internal browser set to the target-market Google domain. Separate business competitors, SERP competitors, directories/aggregators, publishers/forums, and (where active) local/product/category/solution/marketplace competitors. For important queries inspect domains, page types, topical coverage, trust signals, reviews, relevance, conversion elements, internal links, backlinks where available, freshness.

### Local SEO / GBP (only if local modules enabled)

Audit observable GBP completeness, categories, NAP consistency, service areas, hours, website, reviews, photos/posts, Local Pack presence, local landing pages, and citation consistency. Never invent unobserved fields. If local modules are disabled or conditional-untriggered, skip and record that in the summary; do not produce empty audit files.

### AI search competitors

If `ai_search_visibility` is enabled: identify which competitors and third-party sources AI answers cite for key queries and what they share (structure, direct answers, entity/proof signals, freshness); note consistency of business name, address, services and reviews across listings. Use `02-GOOGLE-RESEARCH/ai-search-observations.csv` and Semrush AI visibility if available.

### Device split

If enabled, compare mobile and desktop SERPs for important queries (always for local intent where the local module is on) and record ranking, feature and Local Pack differences per the Device policy.

### Evidence, not architecture

Record page types and structures observed as `COMPETITOR_OBSERVATION`. Pass them to 05 (intent, cannibalisation) and 06 (architecture), which decide.

## Outputs

- `04-COMPETITOR-LOCAL/COMPETITOR-ANALYSIS.md` (by active competitor type; observations flagged as evidence)
- `04-COMPETITOR-LOCAL/SERP-COMPETITORS.csv` (with `competitor_type`)
- **`04-COMPETITOR-LOCAL/COMPETITOR-KEYWORD-FINDINGS.csv`** (candidate terms/topics from competitor and local evidence; fields in `../SCHEMAS.md` §3)
- `DEVICE-SERP-ANALYSIS.csv`, `BACKLINK-GAP-SUMMARY.md` (if enabled and data exists)
- Local modules only: `LOCAL-SEO-AUDIT.md`, `GBP-AUDIT.md`, `CITATION-AUDIT.md`

## Blocking conditions

Config missing; Google market cannot be verified; local modules enabled but the target location can't be set.

## QA checks

- [ ] Only strategy-active competitor types were researched; business vs SERP (and other) types kept separate.
- [ ] Local audit exists only where local modules are active, and is evidence-based.
- [ ] Competitor architecture recorded as observation, not instruction.
- [ ] Candidate findings carry eligibility, decision, reason and source.
- [ ] Device differences recorded where enabled and material.
- [ ] Backlink gap summarised if enabled and data exists.

## Handoff

`COMPETITOR-KEYWORD-FINDINGS.csv` and competitor analysis to 05 (merge, intent) and 06 (page structure evidence). Gate C/D check.
