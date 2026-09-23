# SEO Keyword Research System — Global Instructions

This repository is a modular SEO keyword-research system. Every specialist agent must be runnable in a completely new Claude Code session. Agents communicate through files, not conversation history.

## Core rules

1. Business first. Never begin large-scale keyword research without a validated business brief.
2. Never invent business facts, metrics, reviews, certifications, prices, claims, or tool results.
3. Client country, language, location, search engine, and device context control research.
4. Separate business competitors from SERP competitors.
5. Keywords are evidence, not copywriting instructions.
6. Do not create one page per keyword.
7. SERP evidence can override an obviously incorrect automated intent label.
8. Preserve source attribution and research dates.
9. Every agent validates inputs and outputs before completion.
10. Each agent owns only its assigned stage.

## Semrush policy

Semrush is the only paid SEO platform assumed by this system. Do not use or pretend to use a Semrush web UI/browser session.

Whenever Semrush data is needed, ask:

> Should I access Semrush via MCP/API, or should I give you the manual Semrush steps and have you bring the data back?

Allowed paths are actual Semrush MCP/API access or manual Semrush instructions plus user-provided exports/data. Never claim account access unless a tool result confirms it.

## Google policy

Use the available internal browser for Google Keyword Planner, Google Trends, autocomplete, PAA, Related Searches, manual SERP analysis, and local-search observations.

Before every Google Keyword Planner keyword-entry batch verify **location, language, and currency**. Multi-keyword input may be newline- or comma-separated. Scroll/paginate result rows as needed.

When testing autocomplete, clear/remove the previous keyword before entering the next one.

## Semrush API unit budgeting

This applies only when Semrush is reached through live API/MCP access
(`execute_report` and related tools), not when giving the user manual UI
instructions.

Treat API/MCP units as a finite project budget. Before expensive calls,
estimate units; prefer targeted calls; reuse retrieved data; record
estimated and actual units when available; stop when balance is
insufficient; ask before material budget consumption; never invent unit
costs.

### Formula

```text
Estimated units = rows requested × unit cost per row (for that report)
```

"Rows requested" is the `display_limit` for row-based reports, or the number
of phrases/domains for batch reports. Sum every planned call in a multi-call
task (e.g. one seed per Keyword Magic Tool pass) into one total estimate
before starting — do not confirm call-by-call after the fact.

### Known per-row costs

| Report (Semrush tool) | Units / row |
|---|---:|
| Batch Keyword Overview (`phrase_these`) | 10 |
| Keyword Overview, single (`phrase_this`/`phrase_all`) | 10 |
| Organic Results (`phrase_organic`) | 10 |
| Domain Organic (`domain_organic`) | 10 |
| Domain Organic, historical | 50 |
| Broad Match Keyword | 20 |
| Paid Results | 20 |
| Related Keywords / Keyword Magic Tool (`phrase_related`) | 40 |
| Phrase Questions (`phrase_questions`) | 40 |
| Keyword Difficulty (`phrase_kdi`) | 50 |
| Keyword Ads History | 100 |

Source: Semrush's own Keyword Reports API documentation
(developer.semrush.com/api/v3/analytics/keyword-reports/). If a report is
not listed, run a single test call with `display_limit=1` (or the smallest
possible request) first, read the actual `api_units`/cost returned, and use
that measured rate for the rest of the estimate — never guess an unlisted
report's cost.

### Tracking remaining balance

No MCP tool can read the account's remaining unit balance directly. To know
the real remaining budget:

1. Ask the user to open Semrush → profile icon → **Subscription info** →
   **API Units** tab, and report the **Balance** and **next renewal date**.
2. Record that balance and when it was reported.
3. For the rest of the session, subtract each confirmed call's actual cost
   (from the report's returned `usage.api_units` or cost field) from the
   last known balance, and keep the user informed of the running remainder.
4. If the balance hasn't been checked this session, treat it as unknown —
   do not assume it's large enough to proceed.

### When to require confirmation

Always show the estimate first. Require explicit user confirmation before
executing when any of the following is true:

- The remaining balance is unknown.
- The single call or summed multi-call estimate exceeds 500 units.
- The task involves more than 3 `execute_report` calls.
- The estimate would consume more than ~10% of the last known balance.

A trivial, already-approved single lookup within a task the user already
confirmed (e.g. re-running one failed seed at the same depth) does not need
re-confirmation — but still state the cost spent.

### Presentation format

Before running, tell the user: what report(s) and how many rows/calls are
planned, the per-row rate and total estimated units, and the last known
remaining balance plus what would be left after. Then wait for explicit
approval before calling `execute_report`. Never run the calls and report
cost afterward as a fait accompli.

Use this reporting format for the manifest/usage log:

| Action | Purpose | Estimated units | Actual units | Balance after | Status |
|---|---|---:|---:|---:|---|

## Evidence labels

Use `OBSERVED`, `CALCULATED`, `INFERRED`, `USER_PROVIDED`, or `UNKNOWN` when useful. Do not turn an inference into a fact.

## Status values

`NOT_STARTED`, `IN_PROGRESS`, `BLOCKED`, `COMPLETE`, `STALE`, `NEEDS_REVIEW`.

## Project state

`00-ORCHESTRATOR/PROJECT-MANIFEST.md` is the shared source of truth. A stage is `COMPLETE` only when required inputs exist, outputs exist, validation passes, and the manifest is updated.

## Ordering flexibility

After Agent 01 is complete, Agents 02, 03, and 04 have no dependency on each
other. This system has no subagent tools, so they run one at a time in
whatever order the user prefers rather than concurrently — but none of them
needs to wait for a specific one of the other two to finish first.

## Fresh-session rule

No agent may require hidden context from a previous chat. If an input is missing, mark the stage `BLOCKED` and name the missing file rather than guessing.
