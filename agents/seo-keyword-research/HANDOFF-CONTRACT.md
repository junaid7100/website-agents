# Inter-Agent Handoff Contract

## Principle

Agents communicate through explicit files. A new session must understand the project without prior chat history.

## Every agent must

1. Read the manifest.
2. Read declared input files.
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

## Data lineage

Major datasets should preserve source, collection date, market/location, language, device where relevant, and notes.

## Failure behavior

If a required input is absent, do not fabricate it. Mark the stage `BLOCKED`, list the missing input in the manifest, and stop the affected workflow.

## Partial completion

Create available outputs, mark incomplete sections, and use `NEEDS_REVIEW` or `BLOCKED` rather than `COMPLETE`.
