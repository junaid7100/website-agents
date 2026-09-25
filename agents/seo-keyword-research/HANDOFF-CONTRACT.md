# Inter-Agent Handoff Contract

## Principle

Agents communicate through explicit files. A new session must understand the project without prior chat history.

## Every agent must

1. Read the manifest and the config files (`business-profile.json`, `research-strategy.json`, `business-relevance-policy.json`).
2. Read declared input files (candidate datasets, not raw files, unless verifying).
3. Validate input completeness.
4. Perform only assigned work.
5. Write declared outputs.
6. Validate outputs.
7. Update the manifest.
8. Record blockers instead of guessing.

## Evidence labels

`OBSERVED`, `CALCULATED`, `INFERRED`, `USER_PROVIDED`, `UNKNOWN`.

## Page Naming Convention (all agents)

Every page has a **page code** and a **page name (slug)**, always used
together as the **page key**: `PAGE-001-homepage`, `PAGE-014-driveway-paving`.

- **Code:** `PAGE-NNN`, assigned once by SEO stage 06 in
  `KEYWORD-TO-PAGE-MAP.csv` (`page_id` column), sequential. Never reused,
  never renumbered. Pages added later (Expansion Mode) take the next number.
- **Name:** lowercase kebab-case slug from the page's final URL segment (the
  homepage is `homepage`). If the URL/name later changes, the code stays; the
  new slug is recorded in `SITE_INDEX.md` and the folder and files are renamed
  together.
- **Folders:** every per-page folder is named with the full page key, e.g.
  `pages/PAGE-001-homepage/`.
- **Files:** every per-page file is prefixed with the page key and a double
  underscore, e.g. `PAGE-001-homepage__FINAL_COPY.md`,
  `PAGE-001-homepage__AI-DESIGN-PROMPT.md`. In agent specs, bare names such as
  `FINAL_COPY.md` or `HANDOFF.md` are document types; on disk they always
  carry the prefix.
- **Project-level files** (`STATUS.md`, `SITE_INDEX.md`, `PAGE_QUEUE.md`,
  `BRAND-GUIDE.md`, etc.) are not per-page and are not prefixed. Indexes and
  queues list the page key, and every file path recorded in them uses the
  prefixed name.

## Staleness

When a source artifact materially changes, downstream artifacts depending on it become `STALE` until refreshed.

## Raw vs candidate

Discovery stages (02–04) preserve raw datasets and produce candidate datasets. Downstream stages consume candidates; raw files are for traceability and audit. A stage may not start merely because raw files exist.

## Confirmation queue

Unknown business capabilities go to `00-ORCHESTRATOR/CLIENT-CONFIRMATION-QUEUE.csv`, not into assumptions. Downstream stages treat open items as `UNKNOWN`.

## Data lineage

Major datasets should preserve every source a record came from (lineage survives deduplication), collection date, market/location, language, device where relevant, and notes.

## Failure behavior

If a required input is absent, do not fabricate it. Mark the stage `BLOCKED`, list the missing input in the manifest, and stop the affected workflow.

## Partial completion

Create available outputs, mark incomplete sections, and use `NEEDS_REVIEW` or `BLOCKED` rather than `COMPLETE`.
