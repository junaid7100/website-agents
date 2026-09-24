# Shared Schemas, Vocabularies and Module Catalogue

Single source for field lists and vocabularies. Agents reference this file instead of repeating it. Rules live in `CLAUDE.md`; this file holds shapes.

## 1. Config files (written by Agent 00, read by every stage)

All live in the project's `00-ORCHESTRATOR/`.

| File | Purpose |
|---|---|
| `business-profile.json` | What type of SEO problem this is (multi-valued, evidence-backed). |
| `research-strategy.json` | Which modules run, how deep, in what priority; scoring profile; negative-term policy. |
| `business-relevance-policy.json` | What counts as CORE / RELEVANT / CONDITIONAL / IRRELEVANT / UNKNOWN for this project. |
| `PROJECT-MANIFEST.md` | The research manifest: stage status, config status, modules, blockers, sources. (No separate `research-manifest.json`.) |
| `CLIENT-CONFIRMATION-QUEUE.csv` | Open questions the client must answer. Any stage appends; Agent 00 tracks. |

Templates for the first three and the queue ship in `00-ORCHESTRATOR/`. Fill in place.

### Profile value shape

Every important profile conclusion is `{ "value": ..., "confidence": "HIGH|MEDIUM|LOW", "evidence": [ {"type": <evidence type>, "note": "..."} ] }`. Missing information stays `"UNKNOWN"` with `LOW` confidence. Never convert missing into assumed.

Multi-valued dimensions (business models, revenue models, conversions, audiences) are lists of `{value, importance, confidence, evidence}`. Hybrids keep every value.

### Evidence types

`CONFIRMED_CLIENT_FACT`, `WEBSITE_FACT`, `TOOL_DATA`, `SERP_OBSERVATION`, `COMPETITOR_OBSERVATION`, `CUSTOMER_VOC`, `AGENT_INFERENCE`, `UNKNOWN`. These refine the base labels in `CLAUDE.md` (`OBSERVED`, `CALCULATED`, `INFERRED`, `USER_PROVIDED`, `UNKNOWN`); either set is acceptable, but never present `AGENT_INFERENCE` as fact.

## 2. Vocabularies

**Importance / priority:** `CRITICAL | HIGH | MEDIUM | LOW | IRRELEVANT | UNKNOWN` (profile characteristics); module priority `CRITICAL | HIGH | MEDIUM | LOW | DISABLED`.
**Module state:** `ENABLED | DISABLED | CONDITIONAL`. **Depth:** `DEEP | STANDARD | LIGHT | CONDITIONAL | NONE`.
**Business eligibility:** `YES | NO | UNKNOWN`. `NO` rejects; `UNKNOWN` goes to `CLIENT CONFIRMATION REQUIRED` and can never become a target keyword unvalidated.
**Business status (seeds):** `CONFIRMED | UNCONFIRMED | NOT OFFERED`.

**Decision states** (use everywhere, always with `decision_reason`):

```
KEEP — CORE | KEEP — SECONDARY | KEEP — LOCAL | KEEP — CONTENT/FAQ
REVIEW — CLIENT CONFIRMATION | REVIEW — SERP
EXCLUDE — IRRELEVANT SERVICE | EXCLUDE — WRONG INTENT | EXCLUDE — WRONG LOCATION
EXCLUDE — WRONG AUDIENCE | EXCLUDE — JOB/CAREER | EXCLUDE — DIY | EXCLUDE — DUPLICATE
```

`KEEP — LOCAL` and the `EXCLUDE — WRONG LOCATION / JOB/CAREER / DIY` states apply only where the project's policy makes them meaningful. `EXCLUDE — DIY` and `EXCLUDE — JOB/CAREER` are never automatic: they require the relevance policy to list DIY / recruitment as out of scope for this project. For a supplier, publisher or e-commerce business DIY may be `KEEP — CORE` or `KEEP — CONTENT/FAQ`. If a project needs another exclusion reason, add it to the policy file rather than reusing a wrong label.

**Intent labels:** `LOCAL TRANSACTIONAL | TRANSACTIONAL | COMMERCIAL | COMMERCIAL INVESTIGATION | INFORMATIONAL | NAVIGATIONAL | EMPLOYMENT | DIY | AMBIGUOUS | IRRELEVANT`. What is *valuable* is decided from the profile (see Agent 05), not from the label.

**Geography class** (only when a location module is active): `LOCAL TOWN | COUNTY/REGION | NEAR ME | NON-GEO | OUTSIDE SERVICE AREA`.

**Final keyword roles:** `PRIMARY | SECONDARY | SUPPORTING | CONTENT | LOCAL | CLIENT CONFIRMATION | SERP REVIEW | REJECT`. Add a role only when a business model genuinely needs one.

**Trend status:** `RISING | STABLE | DECLINING | SEASONAL | TREND DATA INSUFFICIENT | NOT CHECKED`. Insufficient data is never evidence of zero demand.

## 3. Dataset schemas (CSV headers)

Raw datasets are preserved untouched. Candidate datasets are what travels downstream.

**`01-MARKET-SEEDS/SEED-KEYWORDS.csv`** (the seed-families file; do not create a second one):
`seed, seed_family, offering, sub_offering, seed_type, module, commercial_modifier, location_modifier, business_status, source, evidence_type, notes`

**`02-GOOGLE-RESEARCH/keyword-planner.csv`** = raw Google keyword data. **`google-keyword-candidates.csv`**:
`keyword, source, seed_family, location, geography_class, intent, volume, competition, cpc, trend_status, module, business_relevance, business_eligibility, decision, decision_reason, notes`

**`03-SEMRUSH-RESEARCH/keyword-magic.csv`** (+ other report CSVs) = raw. **`semrush-keyword-candidates.csv`**:
`keyword, source, seed_family, competitor_source, volume, kd, cpc, intent, serp_features, device, module, business_eligibility, decision, decision_reason, notes`

**Competitor/market candidate findings** (`04-COMPETITOR-LOCAL/COMPETITOR-KEYWORD-FINDINGS.csv`):
`term_or_topic, observed_on, competitor_type, page_type_observed, observation, module, business_eligibility, decision, decision_reason, notes`

**`05-KEYWORD-INTELLIGENCE/MASTER-KEYWORDS.csv`** (the single master dataset; lineage never lost):
`cluster_id, keyword, normalized_keyword, seed_family, offering, sub_offering, location, intent, volume, kd, cpc, trend_status, serp_features, sources, business_eligibility, business_relevance, serp_overlap_status, serp_checked, trend_checked, device, primary_secondary_status, keyword_role, recommended_page_type, target_page, decision, decision_reason, internal_score, confidence, notes`
(`target_page` is left blank by Agent 05 and filled from Agent 06's map when the master is refreshed.)

**`CLIENT-CONFIRMATION-QUEUE.csv`:**
`id, question, related_keyword_or_cluster, business_capability, reason, potential_seo_impact, affected_module, affected_page_decision, priority, status, raised_by_stage, date_raised, answer, date_resolved`
Status: `OPEN | ANSWERED | WONT_CONFIRM`.

## 4. Research module catalogue

Modules are catalogued, not activated by default. Agent 00 decides state/priority/depth per project in `research-strategy.json`. The list is extensible: to add a module, add a row here, define its activation trigger in the strategy, and let the owning agents consume it through their existing contracts.

| Module | Primary agent(s) | Typically relevant when |
|---|---|---|
| `service_keyword_research` | 01, 02, 03 | offering is services |
| `local_keyword_research`, `location_modifier_research`, `near_me_research` | 01, 02, 03 | geographic scope local/regional, service-area or physical-location dependent |
| `local_pack_analysis`, `google_business_profile_analysis` | 04 (02 observes) | local intent + physical presence or service area |
| `product_keyword_research`, `category_keyword_research`, `product_attribute_research` | 01, 02, 03 | offering is products / catalogue |
| `problem_solution_research`, `informational_research` | 01, 02, 03 | long sales cycle, SaaS, B2B, publishers, or content supports the offer |
| `comparison_research`, `alternative_research`, `feature_research`, `integration_research`, `use_case_research` | 01, 02, 03, 04 | software/complex products with consideration-stage search |
| `price_cost_research`, `review_research`, `brand_search_research` | 01, 02, 03 | price/reviews drive the decision, or brand demand matters |
| `autocomplete_research`, `keyword_planner_research`, `paa_research`, `related_searches_research` | 02 | discovery tools; depth follows the modules they serve |
| `google_trends_research` | 02 | only with a stated research question |
| `competitor_keyword_research`, `keyword_gap_research`, `content_gap_research`, `backlink_gap_research` | 03, 04 | competitors exist that rank for the target space; backlink depth follows competitiveness |
| `serp_analysis` | 02, 04, 05 | always at least LIGHT (needed to resolve ambiguity) |
| `mobile_desktop_serp_comparison` | 02, 03, 04 | device split plausibly matters (local, on-the-go, shopping) |
| `ai_search_visibility` | 02, 03, 04 | queries where AI answers appear and citation is plausible |

**Module entry shape** in `research-strategy.json`: `{ "state": ENABLED|DISABLED|CONDITIONAL, "priority": ..., "depth": ..., "reason": "...", "trigger": "... (for CONDITIONAL)", "dependencies": [ ... ] }`.

**Illustrative activations (examples only; derive from the actual profile).** Local service: local/location/local-pack HIGH-CRITICAL, product research DISABLED, Trends CONDITIONAL. International SaaS: problem/solution, use-case, integration, comparison, alternative HIGH; local, near-me, location, local-pack DISABLED. E-commerce: product, category, attribute CRITICAL-HIGH; local CONDITIONAL. B2B manufacturer: product/service, application/use-case, problem/solution HIGH; local LOW or DISABLED. Hybrid: union of the relevant sets, overlaps resolved explicitly in `reason`.

## 5. Seed module patterns (Agent 01)

Seed families are picked from the modules that are active; these are pattern libraries, not checklists.

- Local service: offering, sub-offering, provider-type synonyms, installation/hire/supply, near me, offering + location, cost/quote, problem-need.
- SaaS: software category, problem, solution, feature, integration, use case, industry, workflow, alternative, competitor, X vs Y, pricing, review, best.
- E-commerce: product, category, subcategory, brand, material, size, colour, attribute, audience, use case, buy/order, price, best, review, comparison.
- Professional service: service, specialist/consultant/firm/agency/advisor, industry, problem, solution, location (if relevant), consultation, cost.
- B2B/manufacturer: product, service, solution, industry, application, technical problem, specification, supplier/manufacturer/wholesale, commercial, use case.

## 6. Negative-term pattern library (conditional)

Never applied as a universal list. Terms are generated per project from the relevance policy. Common patterns to *consider*: recruitment (`jobs, salary, careers, training`), learning (`course, how to become`), DIY, free/template/software (when selling services), equipment purchase (when offering services or hire only), wrong-industry meanings, unsupported offerings. Each pattern is activated only if the policy marks it out of scope, and the same word may be valuable for another business (DIY for a building-materials supplier, "jobs" for a recruiter).

## 7. Optional internal score (Agent 05)

Sorting/triage aid only. Weights come from `research-strategy.json` → `scoring_profile` (dimensions plus weights summing to 100), generated for the project. Never one fixed formula, never a keep/reject or create-page threshold. If score and categorical reasoning conflict, follow the reasoning and investigate why the score misleads. Candidate dimensions: business relevance, intent match, conversion relevance, local/geographic intent, product/category fit, problem/use-case fit, strategic importance, SERP fit, search demand, ranking feasibility, content capability, evidence confidence. Include only those the strategy names.
