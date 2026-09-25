# Expansion Mode — Adding Pages or Opportunities After Handoff

Use when the pipeline has already produced a handoff (Gate F passed) and the
user wants a new page, a new service/product/location, or to chase a new
opportunity (a `Future Opportunity` decision, a confirmed capability, a
Stage 08 finding, or a client idea). This is a **scoped run, not a rerun**.
Do not restart stages 01–07 and do not refresh unrelated datasets.

## Trigger types

1. **New topic/offering** the user names.
2. **Future Opportunity promoted** — its blocking confirmation-queue item was
   answered in `CLIENT-CONFIRMATION-QUEUE.csv`.
3. **Measurement finding** from `08-MEASUREMENT/SEO-OPPORTUNITIES.md`.
4. **Copywriting/design finding** filed as an SEO HANDOFF ISSUE.

## Procedure (permission before each numbered step)

1. **Scope.** Write an `EXPANSION-<NN>-<slug>/` entry under
   `00-ORCHESTRATOR/` with: trigger, the topic/offering, business-eligibility
   check against `business-relevance-policy.json`, and which existing
   datasets already cover it. Update the profile/strategy only if the
   business facts changed (then mark affected outputs `STALE` per the normal
   rule).
2. **Reuse first.** Search existing `CLEAN-KEYWORDS.csv`, clusters and
   `KEYWORD-TO-PAGE-MAP.csv` for the topic. If existing evidence already
   answers it, skip to step 4.
3. **Targeted research only for gaps**, following each stage's own rules and
   permission gates (Semrush access is asked again every time). Research
   only the new topic; keep raw and candidate files separate, preserve
   lineage, and append to (never overwrite) the existing datasets.
4. **Merge and cluster** the new candidates into the existing clusters via
   the Stage 05 method, SERP-validating anything ambiguous.
5. **Page decision (Stage 06 rules).** Run the universal page-creation test
   and check cannibalisation against *every existing page and its primary
   keyword owner*. Outcomes: `NEW PAGE`, `EXTEND EXISTING PAGE` (the owner
   page's brief changes), `NO DEDICATED PAGE`, or `FUTURE OPPORTUNITY`. Log
   the reasoning in `ARCHITECTURE-DECISIONS.md`.
6. **Append to the architecture:** new rows in `KEYWORD-TO-PAGE-MAP.csv` and
   `WEBSITE-PAGE-INVENTORY.md`, a new brief in `PAGE-BRIEFS/`, and a
   **link-impact list**: existing pages that should link to the new page
   (and anchor context) and any existing brief whose ownership or internal
   links change. Existing URLs are never changed silently.
7. **Refresh the handoff (append mode).** Re-copy changed artifacts into
   `08-COPYWRITING-AGENT/`, add the new page's brief to `08-PAGE-BRIEFS/`,
   and update `12-COPYWRITING-HANDOFF.md`: new page `READY`/`BLOCKED`, a
   dated `EXPANSION` section listing added pages, changed pages and the
   link-impact list. Pages that were already `COMPLETE` downstream are only
   marked "needs link/brief update" — they are not reopened.
8. Update `PROJECT-MANIFEST.md` (expansion entry, date, pages added) and
   report to the orchestrator.

## Rules

- Never create a page from volume alone; never one page per keyword.
- Never fabricate proof; unknown capabilities go to the confirmation queue.
- Existing datasets stay authoritative; new data is appended with lineage.
- If the new topic would materially change ownership of existing pages, stop
  and present the change to the user before editing the map.
