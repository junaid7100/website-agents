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
