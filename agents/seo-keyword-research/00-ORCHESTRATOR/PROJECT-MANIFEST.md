# Project Manifest

> Template. Replace placeholders.

## Project
- Project name: `{{PROJECT_NAME}}`
- Client: `{{CLIENT}}`
- Domain: `{{DOMAIN}}`
- Research date: `{{YYYY-MM-DD}}`

## Market
- Country: `{{COUNTRY}}`
- Region/state/province: `{{REGION}}`
- City: `{{CITY}}`
- ZIP/postcode: `{{ZIP_OR_NA}}`
- Language: `{{LANGUAGE}}`
- Currency: `{{CURRENCY}}`
- Search engine: `{{SEARCH_ENGINE}}`
- SEO scope: `{{LOCAL | NATIONAL | INTERNATIONAL}}`
- Device priority: `{{UNKNOWN | MOBILE | DESKTOP | BOTH}}`

## Current state
- Current stage: `00`
- Overall status: `NOT_STARTED`
- Last completed stage: `NONE`
- Last updated: `{{TIMESTAMP}}`

## Configuration status
| Config file | Status | Last updated | Notes |
|---|---|---|---|
| business-profile.json | `NOT_STARTED` | | |
| research-strategy.json | `NOT_STARTED` | | |
| business-relevance-policy.json | `NOT_STARTED` | | |
| CLIENT-CONFIRMATION-QUEUE.csv | `NOT_STARTED` | | open items: 0 |

## Modules (from research-strategy.json)
| Module | State (`ENABLED/DISABLED/CONDITIONAL`) | Priority | Depth | Reason / trigger |
|---|---|---|---|---|
| | | | | |

## Stage gates
| Gate | Description | Status | Date |
|---|---|---|---|
| A | Business understanding complete | `NOT_STARTED` | |
| B | Seed families approved | `NOT_STARTED` | |
| C | Discovery complete (raw + candidates per active stage) | `NOT_STARTED` | |
| D | Candidate datasets ready for 05 | `NOT_STARTED` | |
| E | Intelligence complete (clusters, roles, SERP-validated) | `NOT_STARTED` | |
| F | Architecture complete (ownership, cannibalisation) | `NOT_STARTED` | |

## Open items
- Client confirmation items open: `{{COUNT}}` (see `CLIENT-CONFIRMATION-QUEUE.csv`)
- SERP review items open: `{{COUNT_OR_LIST}}`
- Pending / blocked agents: `{{LIST}}`

## Data sources and evidence
- Sources used (with dates and market/device): `{{LIST}}`
- Evidence status (which conclusions are CONFIRMED_CLIENT_FACT vs AGENT_INFERENCE): `{{NOTE}}`

## Stage status
| Stage | Agent | Status | Last run | Notes |
|---|---|---|---|---|
| 00 | Orchestrator | `IN_PROGRESS` | | |
| 01 | Market & Seeds | `NOT_STARTED` | | |
| 02 | Google Research | `NOT_STARTED` | | |
| 03 | Semrush Research | `NOT_STARTED` | | |
| 04 | Competitor & Local SEO | `NOT_STARTED` | | |
| 05 | Keyword Intelligence | `NOT_STARTED` | | |
| 06 | Page Architecture | `NOT_STARTED` | | |
| 07 | Copywriting Handoff | `NOT_STARTED` | | |
| 08 | Measurement | `NOT_STARTED` | | |

## Blockers
- `{{NONE}}`

## Missing inputs
- `{{NONE}}`

## Semrush access
- Mode: `NOT_DECIDED | MCP/API | BROWSER | MANUAL` (asked again at each Semrush use; this records the latest choice only)
- Unit budget available: `{{UNKNOWN_OR_VALUE}}`
- Unit budget used: `{{UNKNOWN_OR_VALUE}}`

## Completion rule
A stage is `COMPLETE` only when its output files exist and its validation checklist passes.
