---
name: seo-semrush-research
description: Adaptive Semrush evidence — keyword expansion, competitor keyword discovery, gaps, top pages, SERP intelligence, backlink gap — via MCP/API, the internal browser, or manual export, with API unit budgeting, producing a filtered Semrush candidate dataset. Stage 03 of the SEO pipeline — has no dependency on seo-google-research or seo-competitor-local (order between the three is flexible) after seo-market-seeds completes.
---

# Agent 03 — Semrush Research (Adaptive)

## Role

Use Semrush for keyword expansion, intent/volume/KD/CPC evidence, competitor keyword discovery, SERP analysis, gaps and organic-competitor discovery, and produce a **filtered Semrush candidate dataset**. Evidence, not decisions.

## First action

Ask:

> How should I access Semrush this time: (1) MCP/API, (2) the internal browser (you stay logged in to Semrush there), or (3) manual steps where you bring the data back?

Ask this every time, including on re-runs; do not assume access or reuse a previous choice.

## Inputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`, `business-profile.json`, `research-strategy.json`, `business-relevance-policy.json`
- `01-MARKET-SEEDS/SEED-KEYWORDS.csv`

Optional: competitor domains, Google research outputs.

## Business context required

Context Adaptation per `../CLAUDE.md` first. Which Semrush capabilities run, and how deep, comes from the strategy: not every business needs every report (e.g. backlink gap may be LIGHT for a local contractor and DEEP for a competitive SaaS category; keyword gap may be the priority for an e-commerce catalogue). Database/country follows the target market.

## Active modules

`keyword_planner`-equivalent expansion via Keyword Magic for the enabled seed-family modules, `competitor_keyword_research`, `keyword_gap_research`, `content_gap_research`, `backlink_gap_research`, `serp_analysis`, `mobile_desktop_serp_comparison`, `ai_search_visibility`. Skip any that are `DISABLED`; run `CONDITIONAL` ones only on a logged trigger.

## Responsibilities

Expand seed families in Keyword Magic; collect metrics; discover real organic competitors; extract competitor keywords and run them through the same relevance/eligibility gates; identify gaps; capture top pages, SERP features, AI visibility; log unit usage; produce raw and candidate datasets.

## Decision authority

May: filter, classify intent, mark business eligibility (`YES/NO/UNKNOWN`), assign provisional decision states, and label gap keywords.
**Forbidden:** blind export of every keyword; universal minimum-volume / maximum-KD / minimum-CPC thresholds; treating a competitor's ranking as proof of relevance or of a client capability; assuming the client's named competitor is the only SEO competitor; primary/secondary selection, clustering, or page decisions (Agents 05/06).

## Workflow

### Tool roles and access

Semrush is the **primary** tool in this stage; Google research is supporting context only. Follow the Semrush policy in CLAUDE.md (access path chosen each time; unit budgeting for API/MCP; extraction rules for browser mode).

### Browser extraction (no export)

If the user chooses the internal browser, follow "Semrush browser extraction without export" in CLAUDE.md: apply filters first (database, intent, include/exclude terms, offering and location terminology *only where those modules are enabled*, competitor/page scope) so only needed rows are shown; read the table on screen; write CSV rows into `03-SEMRUSH-RESEARCH/`; save per page; stop when rows stop being relevant; verify against "Total results"; checkpoint after each seed/report. If a report can't be read or a plan cap blocks it, ask the user to supply that report as a file in this folder; never request Downloads access. Apply the 300-row large-pull rule.

### Device

Follow the Device policy in CLAUDE.md. For priority keywords and competitors pull desktop and mobile separately where a report offers a device option, add a `device` column, and note where a report has no split. Skip if `mobile_desktop_serp_comparison` is disabled.

### Keyword Magic

Begin from Agent 01 seed families. Use the correct database. Preserve keyword, volume, KD, intent, CPC, SERP features, trend. Then screen every row through these gates before it reaches the candidate file:

1. **Relevance:** remove unrelated meanings, unsupported offerings, wrong industries/products/brands, unrelated locations (per policy).
2. **Intent:** classify per SCHEMAS.md. Useful informational queries are not deleted; they become `KEEP — CONTENT/FAQ`. Their value is interpreted through the profile's intent priorities.
3. **Business eligibility:** `YES | NO | UNKNOWN`. `UNKNOWN` never becomes a target without validation → `REVIEW — CLIENT CONFIRMATION` + queue.
4. **Metrics:** record volume, KD, CPC, intent, SERP features, trend where available. No universal thresholds; metrics inform prioritisation in 05, not eligibility.

### Competitors and gaps

Identify actual organic competitors from SERPs/Semrush, not just the client's named competitors, and keep the competitor types the strategy activates (business, SERP, content, product, category, solution, marketplace). Extract competitor keywords, then run them through the **same** relevance and eligibility gates. Keyword Gap (client vs roughly 3–5 strong competitors where appropriate) surfaces missing commercial topics, missing sub-offerings, missing supporting content, competitor advantages. Label gap items `CORE OPPORTUNITY | SECONDARY OPPORTUNITY | CONTENT OPPORTUNITY | IRRELEVANT | CLIENT CONFIRMATION REQUIRED`.

### Organic Research / Top Pages

For major competitors capture keyword, position, volume, KD, intent, URL, traffic estimate where available, SERP features, domain, device where split. For top pages capture URL, page type, topic cluster, ranking terms, business role. Competitor pages are evidence, not a blueprint.

### Backlink gap

Where enabled and available, compare referring domains and target pages. Record competitor(s), target page, link context/type, relevance, replicability, notes. Opportunity evidence, not a guaranteed outreach list.

### AI search visibility

Where enabled and the account/path exposes it, capture keywords triggering AI Overviews, cited domains, and client vs competitor AI visibility to `ai-visibility.csv`. If the account lacks the reports, record that after a real check; do not estimate.

### API unit budgeting

Estimate units before expensive calls; reuse data; avoid redundant calls; log estimated/actual units and balance; in browser mode log filters, sort, pages read, rows captured vs. "Total results", and any cap hit; stop when insufficient; ask before material budget consumption; never invent unit costs.

## Outputs

- Raw: `03-SEMRUSH-RESEARCH/keyword-magic.csv`, `keyword-gap.csv`, `organic-research.csv`, `top-pages.csv`, `backlink-gap.csv`, `ai-visibility.csv` (where available)
- **Candidates: `03-SEMRUSH-RESEARCH/semrush-keyword-candidates.csv`** (fields in `../SCHEMAS.md` §3; competitor and gap keywords carry `competitor_source`)
- `SEMRUSH-USAGE-LOG.md`, `RESEARCH-SUMMARY.md` (capabilities run/skipped and why; filters; raw vs candidate counts)

## Blocking conditions

Config or seeds missing; no access path selected; wrong database/country; unit balance unknown and estimate requires confirmation; >300-row pull not approved.

## QA checks

- [ ] Access mode selected this run; correct database/country; usage logged.
- [ ] Only strategy-enabled capabilities ran; depth matches strategy.
- [ ] Raw preserved; candidate file has eligibility, decision and reason on every row.
- [ ] Competitor and gap keywords went through the same gates.
- [ ] No universal volume/KD/CPC threshold used; no fabricated metrics.
- [ ] Reports over 300 rows were raised before extraction; browser mode logged filters, pages, captured vs total, caps.
- [ ] Unknown capabilities queued.

## Handoff

`semrush-keyword-candidates.csv` (primary), raw reports, competitor/organic-competitor list, and summary to 05 (and competitor list to 04). Gate C/D check.
