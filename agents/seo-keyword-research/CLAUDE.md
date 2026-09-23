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
11. Never conclude a tool, site, or capability is blocked, unavailable, or
    off-limits from assumption. Actually attempt the call/action (browser
    navigation, MCP tool call, file read) and read the real result before
    telling the user it doesn't work or falling back to asking them to do
    it manually. Only fall back to a manual path after an actual attempt
    fails or a real check confirms it — not from a guess about what's
    likely to be blocked.
12. Missing required information is not automatically `UNKNOWN`. Before
    marking anything unavailable: (a) decide whether it can reasonably be
    obtained through internet/browser research; (b) if so, ask the user's
    permission to use the internet browser to research it; (c) once
    approved, do the research and populate the field from what you find,
    labeled `OBSERVED`/`INFERRED` as appropriate; (d) only mark something
    genuinely `UNKNOWN` when it can't be determined from research or
    supplied inputs. This applies to every stage, not just customer
    voice/language in Agent 01 — see that agent's own instructions for the
    specific research scope required there.
13. No agent in this pipeline dispatches Task/Agent tool subagents. Every
    stage's research and processing happens inside the current session by
    Reading that stage's own `AGENT.md` and following it directly — see
    "How stages actually run" in `00-ORCHESTRATOR/AGENT.md`.

## Semrush policy

Semrush is the only paid SEO platform assumed by this system. Do not use or pretend to use a Semrush web UI/browser session.

Whenever Semrush data is needed, ask:

> Should I access Semrush via MCP/API, or should I give you the manual Semrush steps and have you bring the data back?

Allowed paths are actual Semrush MCP/API access or manual Semrush instructions plus user-provided exports/data. Never claim account access unless a tool result confirms it — and never assume MCP/API access is *missing* either; check for the actual Semrush tools before telling the user it isn't available.

## Google policy

Use the internal browser for Google Keyword Planner, Google Trends, autocomplete, PAA, Related Searches, manual SERP analysis, and local-search observations. Before concluding the browser can't reach a site or perform an action, actually try it once — don't assume a login wall, region block, or bot check exists without seeing it happen.

Always use the Google country/domain (e.g. google.com vs. google.co.uk vs.
google.com.au) that matches the business's actual target market/geography,
not the default the browser happens to open with. Set it explicitly before
researching, and record which country/domain was used alongside the
results so downstream stages know what market the SERP evidence reflects.

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
