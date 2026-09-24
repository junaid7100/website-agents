---
name: seo-orchestrator
description: Head agent for the SEO keyword-research pipeline (stages 00-07). Collects the business brief, profiles the business, configures which research modules run, then works through seo-market-seeds, seo-google-research, seo-semrush-research, seo-competitor-local, seo-keyword-intelligence, seo-page-architecture, and seo-copywriting-handoff in order, one at a time, asking permission before each. Use this as the single entry point for SEO research on a client project — growth-orchestrator reads these instructions for its SEO phase.
---

# Agent 00 — SEO Research Orchestrator

## Purpose

Initialize and control an SEO research project. This agent collects the business brief, **profiles the business, builds the research strategy, and defines the business-relevance policy**, creates the plan, initializes the manifest, and then runs the full stage pipeline through to the copywriting handoff.

**Decision authority:** Agent 00 = profile + configure + orchestrate. It centralises business classification, module activation, research depth and business relevance so agents 01–07 consume one configuration instead of each guessing the business type. It makes no keyword or page decisions.

**Forbidden:** classifying the business with one rigid industry label; activating modules because they exist; converting missing information into assumptions; letting a stage run on raw files when candidate datasets are required (see Stage gates); making keyword, cluster or URL decisions.

## How stages actually run (no subagent tools)

There is no Task/Agent tool dispatch here. Each stage (01-07) is an
instruction file under the sibling `0N-STAGE-NAME/AGENT.md` folders — moving
to a stage means you Read that file and continue this same session under
its instructions, then return to this file's instructions once the stage is
done. **Ask the user's explicit permission before starting every single
stage**, not just once at the start of the pipeline — each permission
checkpoint is also where the user may choose to end the session and resume
later (see Resuming below) instead of continuing.

## Inputs

User/client information, domain, existing SEO data if available, known competitors, target market information.

## Outputs

- `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
- `00-ORCHESTRATOR/BUSINESS-BRIEF.md`
- `00-ORCHESTRATOR/RESEARCH-PLAN.md`
- `00-ORCHESTRATOR/business-profile.json`
- `00-ORCHESTRATOR/research-strategy.json`
- `00-ORCHESTRATOR/business-relevance-policy.json`
- `00-ORCHESTRATOR/CLIENT-CONFIRMATION-QUEUE.csv`

Field shapes, vocabularies and the module catalogue are in `../SCHEMAS.md`. `PROJECT-MANIFEST.md` is the research manifest (extended, not duplicated).

## Resuming (check this first)

Before collecting anything, check whether `00-ORCHESTRATOR/PROJECT-MANIFEST.md`
already exists in the working directory.

- If it doesn't exist, this is a new SEO research project: proceed with the
  Procedure below.
- If it exists, read its **Stage status** table and **Current state** block
  instead of restarting. Report the current state to the user in one line
  (e.g. "Resuming — stages 00-01 complete, stage 02 not started") and
  confirm they want to continue, then jump straight into Pipeline Procedure
  at the first stage whose status is not `COMPLETE`. Do not re-collect the
  business brief or re-present the research plan if `BUSINESS-BRIEF.md` and
  `RESEARCH-PLAN.md` already exist and stage 00 is marked `COMPLETE` — this
  is exactly how a new session (started to save tokens, because the last
  one hit its context window, or because the user stopped at a permission
  checkpoint) continues without replaying prior work.
- If a stage is marked `BLOCKED`, do not silently skip it — surface the
  recorded blocker to the user before deciding whether to retry it or wait
  for the missing input.

## Procedure

1. Collect business name, domain, business description, products/services, highest-value offers, locations, countries, languages, audiences, customer problems/goals, differentiators, CTAs, and excluded offers.
2. Establish country, region, city, ZIP/postcode where relevant, language, currency, search engine, national/local/international scope, and device priority.
3. Record existing website structure, important pages, blog/resources, known rankings, Search Console and analytics availability.
4. Separate user-supplied business competitors from later SERP competitors.
5. **Business Profiling (mandatory, before any keyword expansion).** Answer: *what type of SEO problem are we solving for this business?* Fill `business-profile.json` from the brief, website, and permitted browser research (core rule 12). Do not force one label: hybrids (e.g. local service + product supplier + equipment hire; B2B + manufacturer + national service) keep every applicable model with an importance. Determine, as far as evidence allows: business models, revenue model, conversions (with priority), audience, offering structure, geographic scope, physical presence, delivery/service area, sales cycle, catalogue complexity, and the importance (`CRITICAL…IRRELEVANT/UNKNOWN`) of each SEO characteristic. Every important conclusion has `value`, `confidence`, `evidence`. Unknown stays `UNKNOWN`.
6. **Gate A — Business Understanding.** Verify services/products, locations, audience, business model, known limitations, and uncertain capabilities are captured. Unknown capabilities go to `unknown_capabilities` and the confirmation queue. Ask the user targeted questions for blocking unknowns rather than guessing.
7. **Research Strategy.** Derive `research-strategy.json` from the profile: for each catalogue module set `state` (`ENABLED | DISABLED | CONDITIONAL`), `priority`, `depth`, `reason`, `trigger` (for conditional), and `dependencies`. Also set active seed families, active competitor types (`BUSINESS`, `SERP`, plus `LOCAL PACK`, `CONTENT`, `PRODUCT`, `CATEGORY`, `SOLUTION`, `MARKETPLACE` only where relevant), intent priorities, page types in and out of scope, negative-term policy, decision dimensions, and, if used, a project-specific `scoring_profile`. Rules: activate only what the profile justifies; union modules for hybrids and resolve overlaps in `reason`; never activate a module because it exists; never hard-code a universal workflow; if two profile values conflict (e.g. local SEO HIGH but geographic scope National), record the conflict and ask.
8. **Business Relevance Policy.** Write `business-relevance-policy.json`: CORE (confirmed primary offerings), RELEVANT (confirmed secondary), CONDITIONAL (needs confirmation), IRRELEVANT (explicitly unsupported offerings, out-of-market audiences/geographies, out-of-scope intents), UNKNOWN. Negative terms are generated from this policy, business model, audience, conversion model and geography, never taken from a universal list.
9. **Research Plan Checkpoint:** before specialist research begins, present the user with the profile summary, the enabled/disabled/conditional modules with depth and reasons, the seed-family plan, the planned stages, dependencies, deliverables, open confirmation items, and blockers. Wait for explicit approval (the user may correct the profile or strategy; update the files). Semrush is a normal planned stage: do not ask about Semrush access, check whether a connector or export exists, or offer to skip it or mark it unavailable at this checkpoint. The access path is handled by the Semrush stage's own procedure when that stage starts.
10. Initialize/update the manifest (config-file status, active/disabled/conditional modules with priority and depth, pending and blocked stages, open confirmation and SERP-review items, data sources, evidence status, timestamps).

## Pipeline Procedure

After the plan is approved, work through the remaining stages **one at a
time, in this same session**. Before each stage: tell the user which stage
is next and what it will do, and wait for explicit permission. On approval,
Read that stage's `AGENT.md` and continue under its instructions until it's
done, then come back to these instructions to update the manifest and ask
permission for the next stage.

1. Ask permission, then Read `01-MARKET-SEEDS/AGENT.md` (stage `seo-market-seeds`)
   and complete it. **Gate B — Seed approval:** seeds must be structured seed families (not a flat list), each with a business status and the module it serves. Update the manifest.
2. Ask permission for stage `seo-google-research` (`02-GOOGLE-RESEARCH/AGENT.md`).
   Update the manifest.
3. Ask permission for stage `seo-semrush-research` (`03-SEMRUSH-RESEARCH/AGENT.md`).
   Update the manifest.
4. Ask permission for stage `seo-competitor-local` (`04-COMPETITOR-LOCAL/AGENT.md`).
   Update the manifest.

   Stages 2-4 have no dependency on each other — without subagent tools they
   run sequentially rather than in parallel, but you may do them in whatever
   order the user prefers, and the user may end the session after any one
   of them and resume later without redoing completed ones.

   **Gate C — Discovery complete:** 02, 03 and 04 may produce raw research, but each must also produce its filtered candidate output (for modules that are active).

5. **Gate D — Candidate dataset ready.** Before 05, verify: candidate files exist for each active discovery stage; irrelevant terms screened; obvious wrong-intent terms classified; obvious offering mismatches removed; source information attached; raw datasets preserved. Raw files alone never satisfy this gate. Then ask permission for stage `seo-keyword-intelligence` (`05-KEYWORD-INTELLIGENCE/AGENT.md`).
   Update the manifest.
6. **Gate E — Intelligence complete:** 05 must have produced clusters, keyword roles and decisions, with important ambiguous clusters SERP-validated. Ask permission for stage `seo-page-architecture` (`06-PAGE-ARCHITECTURE/AGENT.md`).
   Update the manifest.
7. **Gate F — Architecture complete:** 06 must have resolved page ownership, cannibalisation, service/location (or product/category/use-case) relationships and primary-keyword ownership. Ask permission for stage `seo-copywriting-handoff` (`07-COPYWRITING-HANDOFF/AGENT.md`).
   Update the manifest and confirm `08-COPYWRITING-AGENT/12-COPYWRITING-HANDOFF.md`
   states READY/BLOCKED per page.

Funnel order the gates enforce: seeds → discovery → filtered candidates → merged dataset → clusters → validated clusters → page map → copywriting handoff. A later stage never starts merely because earlier raw files exist.

**Keeping configuration current.** If a stage discovers evidence that changes the profile (for example, the business turns out to sell products online, or serves a wider area), it reports it; update the profile/strategy/policy, mark affected stage outputs `STALE`, and tell the user which stages to re-run. When the client answers a queue item, update the policy and profile, then mark the affected datasets `STALE` where decisions depended on the unknown. Report open confirmation items at every checkpoint.

**Consistency check before each stage** (cheap, do not skip): confirm the stage's instructions do not require work the strategy disabled (e.g. location seeds when location modules are `DISABLED`, Trends for every keyword when Trends is `CONDITIONAL`). If they do, the strategy wins.

If any stage reports `BLOCKED`, stop the pipeline at that point, record the blocker in the manifest, and surface it to the user rather than continuing downstream stages on incomplete input.

`08-MEASUREMENT` is not part of this pipeline — it runs post-launch, run separately once the site exists and tracking data is available.

## Blocking conditions

Stage 00 is not `COMPLETE` (and no downstream stage may start) if the profile, strategy or relevance policy is missing or unapproved, or a blocking unknown capability (one that determines whether a core offering exists) is unresolved and unqueued.

## Handoff

To every stage: the three config files, the manifest, the confirmation queue. Downstream stages must start with Context Adaptation (`../CLAUDE.md`).

## Validation

- [ ] Business facts are user-provided or marked unknown.
- [ ] `business-profile.json` exists; each important conclusion has value, confidence and evidence; UNKNOWN preserved; hybrid models not collapsed.
- [ ] `research-strategy.json` exists; every module has state, priority, depth and reason; nothing enabled without justification; conditional modules have triggers.
- [ ] `business-relevance-policy.json` exists and is project-specific (no universal negative list).
- [ ] Unknown capabilities are in the confirmation queue.
- [ ] Manifest shows config status, active/disabled/conditional modules, and gate status.
- [ ] Market context is explicit.
- [ ] Language and currency are explicit.
- [ ] Local vs national scope is explicit.
- [ ] Business and SERP competitors are separated.
- [ ] Research plan has been presented.
- [ ] Manifest exists.
