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

Semrush is the only paid SEO platform assumed by this system.

**Ask every time.** Each time Semrush data is needed (every stage and every new batch of work, not once per project), ask the user which access path to use, and do not reuse an earlier answer:

> How should I access Semrush this time: (1) MCP/API, (2) the internal browser (you stay logged in to Semrush there), or (3) manual steps where you bring the data back?

Allowed paths: (1) actual Semrush MCP/API access; (2) the internal browser session, only after the user picks it, working in the Semrush web UI they are signed in to (never enter credentials, and record what was observed on screen, not inferred); (3) manual Semrush instructions plus user-provided exports/data. Never claim account access unless a tool result confirms it — and never assume MCP/API access is *missing* either; check for the actual Semrush tools before telling the user it isn't available.

### Semrush browser extraction without export

The user's Semrush plan may not allow exports. When the user picks the
internal browser path, do not rely on Export or clipboard Copy. Instead:

1. **Filter first; capture only what the work needs.** Decide which rows the
   stage actually needs (relevant intent, geography, volume/KD range,
   include/exclude terms, question or word-count filters, competitor or page
   scope) and apply those filters in Semrush before reading anything. Never
   pull a whole report by default. Set the database/country, filters, and
   sort; record them. Set rows-per-page to the maximum offered.
2. Read the rendered table (`get_page_text` / `read_page`), using the
   tool's pagination (or scrolling) whenever the filtered results span more
   than one page, and
   write the rows straight into the stage's CSV inside the project folder
   (e.g. `03-SEMRUSH-RESEARCH/keyword-magic.csv`). Save after every page so a
   broken session loses nothing. Dedupe on keyword.
3. Read only the filtered result set. If it is still large, tighten the
   filters rather than paging through everything; stop when the rows are no
   longer relevant to the stage's need, and say why.
4. Verify the captured row count against Semrush's "Total results" figure and
   record captured vs. skipped. If the plan caps rows or daily results, log
   the cap and treat the data as a sample, not complete.
5. Never write a value that was not seen on screen; leave missing fields
   blank. Capture hover/drill-down fields only for priority keywords.
6. Work at a normal pace. If a throttle, bot check, or logout actually
   appears, stop and tell the user (never enter credentials or solve a
   CAPTCHA).
7. **Checkpoint** after each seed/report: stop, report rows saved, filters
   used, and any cap hit, and continue only when the user says so.

Files are only ever read from the project folder or from an exact file the
user names or attaches. Never browse or request access to the user's
Downloads folder.

**User-supplied data fallback.** If the browser cannot read a report
reliably or a cap blocks the pull, stop and give the user the exact
report, filters, and target file name. The user saves the file into the
stage folder (e.g. `03-SEMRUSH-RESEARCH/`) or attaches it, then says "done".
The agent then reads that file, checks the columns and row count, and
continues. Offer this for that one report only, not the whole stage.

## Google policy

Use the internal browser for Google Keyword Planner, Google Trends, autocomplete, PAA, Related Searches, manual SERP analysis, and local-search observations. Before concluding the browser can't reach a site or perform an action, actually try it once — don't assume a login wall, region block, or bot check exists without seeing it happen.

Always use the Google country/domain (e.g. google.com vs. google.co.uk vs.
google.com.au) that matches the business's actual target market/geography,
not the default the browser happens to open with. Set it explicitly before
researching, and record which country/domain was used alongside the
results so downstream stages know what market the SERP evidence reflects.

Before every Google Keyword Planner keyword-entry batch verify **location, language, and currency**. Multi-keyword input may be newline- or comma-separated. Scroll/paginate result rows as needed.

When testing autocomplete, clear/remove the previous keyword before entering the next one.

**Large-pull rule (300 rows).** After filtering, check the result count
before extracting. If a single report or seed batch would exceed **300
rows**, do not start extracting. Tell the user the count, the filters
applied, and the options, then wait for their choice: (a) tighten the
filters further, (b) extract only the top 300 by a stated sort, (c) extract
everything in the browser anyway, accepting the token cost, or (d) the user
supplies the data as a file in the stage folder. Also stop and ask if the
count only becomes known mid-extraction and the 300 mark is crossed.

### Keyword Planner extraction without download

Apply the same approach as Semrush browser extraction. Do not rely on
Keyword Planner's Download/export, which lands in the user's Downloads
folder, or on clipboard copy:

1. **Filter first; capture only what the work needs.** After the location,
   language, and currency checks, apply Keyword Planner filters (keyword
   text include/exclude, monthly search range, competition, brand/non-brand
   exclusions, ideas vs. supplied seeds) so only relevant rows are shown.
   Record the filters and sort. Never pull the full idea list by default.
2. Read the rendered table (`get_page_text` / `read_page`), using the
   tool's pagination (or scrolling) whenever the filtered results span more
   than one page, and write rows straight into
   `02-GOOGLE-RESEARCH/keyword-planner.csv` in the project. Save after each
   batch and dedupe on keyword.
3. Verify captured rows against the result count shown, log captured vs.
   skipped, and never write a value not seen on screen (ranges such as
   "1K-10K" are recorded as shown, not converted to exact numbers).
4. Checkpoint with the user after each seed batch: report rows saved and
   filters used, then continue when told.
5. If the table cannot be read or access is blocked after a real attempt,
   tell the user the exact filters and target file name; the user saves the
   file into `02-GOOGLE-RESEARCH/` or attaches it and says "done". Never
   browse or request access to Downloads.

**Large-pull rule (300 rows).** After filtering, check the result count
before extracting. If a single report or seed batch would exceed **300
rows**, do not start extracting. Tell the user the count, the filters
applied, and the options, then wait for their choice: (a) tighten the
filters further, (b) extract only the top 300 by a stated sort, (c) extract
everything in the browser anyway, accepting the token cost, or (d) the user
supplies the data as a file in the stage folder. Also stop and ask if the
count only becomes known mid-extraction and the 300 mark is crossed.

## Tool roles by stage (Semrush vs Google vs first-party)

Each tool has a defined role per stage. Semrush is access-gated by the
Semrush policy above (MCP/API, internal browser, or user-supplied exports, as chosen by the user each time); Google tools use the internal browser per the Google policy.

| Stage | Semrush | Google / first-party |
|---|---|---|
| 01 Market & Seeds | Not primary. Do not use for seed discovery. | Client brief, customer language, Google discovery (autocomplete, PAA, related searches). |
| 02 Google Research | Not used here; may cross-check results later in 05. | Primary: Keyword Planner, Trends, Autocomplete, PAA, Related Searches, live SERPs for the target market. |
| 03 Semrush Research | **Primary**: Keyword Magic, Keyword Overview, Keyword Gap, Organic Research, Top Pages, SERP features, Backlink Gap. | Supporting only. |
| 04 Competitor & Local | Analyse competitor SERPs, top pages, rankings, GBP, NAP, reviews, listings, map tracking (Listing Management / Map Rank Tracker where the account has them). | Verify important live local results and Local Pack in the target-market Google SERP. |
| 05 Keyword Intelligence | Metrics and competitor data. Semrush intent is an input only. | Combine with Google evidence; check important intents against actual SERPs. |
| 06 Page Architecture | Supporting evidence only: clusters, top pages, SERP features, competitor page structures. Never create a page from volume alone. | SERP/local/VOC evidence. |
| 07 Copywriting Handoff | **Not used live.** Work from the approved keyword/page brief; client facts and proof stay authoritative. | Not used live. |
| 08 Measurement | Rankings, backlinks, site health, competitor monitoring. | Google Search Console, GA4, and GBP data for first-party performance and enquiries. |

AI search (Google AI Overviews and answer engines) is observed in stages 02/04/08 via the internal browser and, in 03/08, Semrush AI visibility reports where available; see the AI search policy below.

When Semrush and Google/first-party data disagree, first-party data wins
for performance and enquiries; live SERPs win for intent and local results.

## AI search (GEO/AEO) policy

AI search visibility is part of SEO in this system. "AI search" means Google
AI Overviews and AI Mode, plus answer engines such as ChatGPT search,
Perplexity, Gemini, and Copilot. Rules for every stage that touches it:

- Observe, don't assume. Record AI answers only from what was actually seen
  (internal browser for live answers, or Semrush AI/visibility reports where
  the account has them and the user chose that access path). Log engine,
  query, market, date, whether an answer appeared, which sources were cited,
  and whether the client or competitors were named.
- Never invent citations, mentions, or "AI visibility" scores. If an engine
  is not reachable or requires login, say so after a real attempt (rule 11).
- AI answers are volatile: treat a single observation as a sample, not a
  ranking, and date-stamp everything.
- Optimise for being the clearest, most citable source: direct answers to
  real questions, entity clarity (who/what/where), verifiable facts, sound
  structure, and crawlability by AI crawlers. Do not promise or guarantee
  AI citations, and do not use manipulative prompt-injection-style tactics.

## Device policy (desktop and mobile)

Desktop and mobile can return different SERPs, rankings, SERP features, Local
Pack results, and even intent, so every stage that researches keywords or
SERPs considers both and records which device each observation is for.

- **Google (internal browser):** check important queries on desktop and on a
  mobile viewport (use the browser's mobile emulation, then reset to desktop
  afterwards). Record device on every SERP, autocomplete, PAA, Local Pack, and
  AI-answer observation. Do not assume the two match.
- **Semrush:** wherever a report offers a device option (for example
  Organic Research, Keyword Overview / SERP analysis, Position Tracking, Site
  Audit), pull desktop and mobile separately for priority keywords and
  competitors, and label the device in every row. Where a metric (such as
  Keyword Magic volume) has no device split, say so rather than implying one.
- **Keyword Planner:** use its device breakdown where offered; otherwise note
  that volume is not split by device.
- **First-party (stage 08):** GSC and GA4 device splits are authoritative.
- Store device as an explicit column/field (`device`: `desktop | mobile |
  both | not_split`). Never merge desktop and mobile rows silently; when they
  differ, record both and note the difference.
- Keep the token cost sensible: run the device comparison on priority
  queries and competitors, not on every keyword, and apply the large-pull rule
  to each device pull separately.

## Semrush API unit budgeting

This applies only when Semrush is reached through live API/MCP access
(`execute_report` and related tools), not when using the internal browser or giving the user manual UI
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
