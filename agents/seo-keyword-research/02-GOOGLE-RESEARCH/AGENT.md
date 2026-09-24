---
name: seo-google-research
description: Adaptive Google evidence — discovers search language and expands demand via Keyword Planner, Autocomplete, PAA, Related Searches, selective Trends, and live SERPs, then produces a filtered Google candidate dataset. Stage 02 of the SEO pipeline — has no dependency on seo-semrush-research or seo-competitor-local (order between the three is flexible) after seo-market-seeds completes.
---

# Agent 02 — Google Research (Adaptive)

## Role

Use Google's ecosystem to discover search behaviour and produce a **filtered Google candidate dataset**. Evidence, not decisions. Do not simply collect everything Google returns.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`, `business-profile.json`, `research-strategy.json`, `business-relevance-policy.json`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`, `CUSTOMER-LANGUAGE.md`

## Business context required

Context Adaptation per `../CLAUDE.md` first. Query patterns and depth follow the strategy: local service → service terminology, service + location, near me, provider synonyms, cost/quote, Local Pack; SaaS → category, problem, solution, feature, integration, use case, alternative, comparison, pricing, review; e-commerce → product, category, subcategory, brand, attribute, buy/order, best, review, comparison, product/category SERPs; B2B → technical and industry terminology, applications, solutions, suppliers/manufacturers, use cases. Only what is enabled.

## Active modules

`autocomplete_research`, `keyword_planner_research`, `paa_research`, `related_searches_research`, `google_trends_research` (conditional), `serp_analysis`, `mobile_desktop_serp_comparison`, `ai_search_visibility`, plus the seed-family modules (service, local, product, category, problem/solution, comparison, etc.) and `local_pack_analysis` observations only if enabled.

## Responsibilities

Discover terminology, modifiers, long-tail patterns and questions; expand seed concepts; provide Google demand evidence; screen every Keyword Planner row through the filters below; observe SERP behaviour for representative queries; produce raw and candidate datasets.

## Decision authority

May: assign a **provisional** decision state and reason to each row (business relevance, eligibility, intent, geography class), and screen out obvious mismatches. Screening decisions are re-openable by 05.
**Forbidden:** primary/secondary selection; clustering; creating pages; treating volume as eligibility; running Autocomplete/Trends/SERP checks on every keyword; recursive Autocomplete; applying a negative-term list not derived from this project's policy; concluding zero demand from missing volume or Trends data.

## Workflow

### Tool roles and depth

- **Autocomplete:** discovery of language, modifiers, terminology, long-tail patterns and offering/modifier combinations for *representative seed families*, using targeted modifiers. Clear the previous query before each test. No default A–Z; A–Z only for consumer/informational long-tail discovery where broad discovery helps, and never by default for local trades or local B2B; state mode and reason in the summary. Never recursively expand every suggestion.
- **PAA / Related Searches:** representative commercial or core queries. Capture recurring questions, related terminology, cost/comparison questions, concerns, supporting topics. Classify separately from core keywords; PAA questions are not automatically primary keywords.
- **Keyword Planner:** see below.
- **Trends:** conditional; only for a stated research question (terminology comparison, offering/category comparison, seasonality, direction, regional variation where data suffices). Record the question and result. `TREND DATA INSUFFICIENT` when Trends lacks data, never "no demand". Do not run Trends on every keyword.
- **SERPs:** representative queries for intent, ranking page types, SERP features, directories/aggregators, and (if enabled) Local Pack. Record enough to feed 05; 05 does the overlap validation.

### Google domain and market

Use the target-market Google domain for all Google work in this stage (SERPs, autocomplete, PAA, Related Searches, AI Overviews, Trends region, Keyword Planner location/language), verify the results really reflect that market, and record domain, country, language, and location on every observation. See the Google policy in CLAUDE.md.

### Google Keyword Planner

Use internal browser set to the target-market Google country/domain. Before **every keyword-entry batch** verify location, language, currency. Use newline/comma multi-keyword input; scroll/paginate as needed. Do not rely on Download/export or clipboard: follow "Keyword Planner extraction without download" in CLAUDE.md (filter first, read the table on screen, write rows into `02-GOOGLE-RESEARCH/keyword-planner.csv`, checkpoint after each seed batch; user-supplied file fallback; never touch Downloads). Capture keyword, average monthly searches, competition, CPC, trend, ideas, source seed, date. Volume is demand evidence, not guaranteed traffic; ranges like "1K–10K" are recorded as shown.

**Screen before handing downstream** (each in the context of the profile and policy):

1. *Business relevance/eligibility* — can this business satisfy the intent? `YES | NO | UNKNOWN`; remove obvious mismatches (offerings not provided, wrong industry, unrelated brands); `UNKNOWN` → `REVIEW — CLIENT CONFIRMATION` and queue.
2. *Negative intent and audience* — only patterns the project's policy marks out of scope (e.g. job seekers, students, DIY-only users, equipment buyers when the business supplies services only). The same term may be valuable for another business; check the policy.
3. *Geography* (only if a location module is enabled): `LOCAL TOWN | COUNTY/REGION | NEAR ME | NON-GEO | OUTSIDE SERVICE AREA`. Keep broader non-geo terms; remove clearly out-of-area terms.
4. *Intent* per SCHEMAS.md. Prioritise the intents the strategy's `intent_priorities` names; store informational/content-type keywords as `KEEP — CONTENT/FAQ` rather than discarding.

Do not apply minimum volume, maximum competition/KD or CPC thresholds; zero/low volume is not rejection.

### SERP analysis

For representative and high-value queries record query, market, location, device, date, top URLs, domains, page types, intent, SERP features, Local Pack (if enabled), Shopping, Videos, Images, PAA, AI Overview, Featured Snippet, forums, content formats, common topics, conversion elements, trust elements, freshness. Check important queries on desktop and a mobile viewport (emulate, then reset), record `device` on every observation and any differences (Device policy in CLAUDE.md). Skip the device comparison if `mobile_desktop_serp_comparison` is disabled.

### AI search observation

For important queries (informational, comparison, high-value commercial), if `ai_search_visibility` is enabled, check AI Overview (and AI Mode where reachable): whether it appears, main points, cited sources, whether client/competitors are cited. Where the user approves and engines are reachable, sample ChatGPT search, Perplexity, or Gemini. Output `ai-search-observations.csv`. Follow the AI search policy in CLAUDE.md.

## Filtering / validation rules

- Raw stays untouched (`keyword-planner.csv`); the working set is `google-keyword-candidates.csv`.
- Every candidate row has a decision state, reason, and source; every removed row is recoverable from raw.
- Effort narrows: representative queries only for Autocomplete/PAA/SERP.

## Outputs

- Raw: `02-GOOGLE-RESEARCH/keyword-planner.csv` (raw Google keyword data), `autocomplete.csv`, `paa.csv`, `related-searches.csv`
- **Candidates: `02-GOOGLE-RESEARCH/google-keyword-candidates.csv`** (fields in `../SCHEMAS.md` §3; questions/informational items appear here as `KEEP — CONTENT/FAQ` with their type)
- `trends.csv` (with the research question per row; only if triggered), `serp-observations.csv`, `ai-search-observations.csv`, `RESEARCH-SUMMARY.md` (modules run/skipped and why, filters used, rows raw vs candidate, Trends questions asked)

## Blocking conditions

Config or seeds missing; target-market Google domain cannot be verified; Keyword Planner location/language/currency unverified; a >300-row pull not yet approved by the user.

## QA checks

- [ ] Only enabled modules were run; conditional modules ran only on a logged trigger.
- [ ] Every KWP batch verified location/language/currency; batches over 300 rows were raised with the user; filters recorded; rows captured vs. result count logged.
- [ ] Raw preserved; candidate file created with decision + reason for every row.
- [ ] Filters derived from this project's policy; no volume/KD/CPC threshold applied.
- [ ] Autocomplete not recursive, cleared between tests, mode/reason stated; Trends only for stated questions.
- [ ] Desktop and mobile checked for important queries where enabled; `device` recorded.
- [ ] AI answers recorded for important queries where enabled.
- [ ] Target-market Google domain, country, language, and location used and recorded; source/date retained.
- [ ] Unknown capabilities queued.

## Handoff

`google-keyword-candidates.csv` (primary), raw files for traceability, SERP/AI/PAA observations, and the summary to 05. Gate C/D check.
