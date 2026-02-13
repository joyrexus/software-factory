# Codebase Cartography

*Structured documentation as semantic index.*

## The Technique

[Codebase indexing](codebase-indexing.md) gives agents the ability to *find* code — deterministic search, symbol resolution, cross-repository navigation. But finding code is not the same as understanding a system. An agent can locate every call to `processInvoice()` across ten repositories without understanding the end-to-end flow that triggers it, the state machine governing invoice lifecycle, or the retry semantics when the payment provider fails.

Codebase cartography closes this gap by producing structured documentation artifacts — component maps, flow narratives, contract definitions, operational runbooks — that give agents (and humans) semantic understanding of how the system works, not just where the code lives. If indexing answers "where is this symbol defined?", cartography answers "what does this system do, and how do its parts relate?"

The practitioner evidence for this approach is direct. The [Harness Engineering](../SOURCES.md#harness-engineering) team tried the "one big AGENTS.md" approach and reports it failed in predictable ways: a giant instruction file crowded out the task and relevant code, too much guidance became non-guidance (agents pattern-matched locally instead of navigating intentionally), the monolith rotted instantly as no single person maintained it, and a single blob resisted mechanical verification for coverage, freshness, or cross-linking. Their solution: treat AGENTS.md as a table of contents (roughly 100 lines), not an encyclopedia, and make the repository's structured `docs/` directory the system of record. [Progressive Disclosure](progressive-disclosure.md) covers the general principle; cartography is its concrete implementation for system documentation.

The key design constraint: folders encode intent, not team preferences. A predictable, convention-driven directory structure means agents can navigate documentation the same way across every repository in the organization. The structure itself becomes a queryable interface — an agent looking for "how does invoice generation work?" knows to check `docs/flows/invoice-generation/`, just as it knows to check `docs/components/billing-service/` for the billing service's responsibilities and interfaces.

## The Documentation Architecture

A docs-as-code folder structure that scales from a few services to many:

```
docs/
  overview.md                    — Front door: responsibilities, dependencies, where to start
  getting-started.md             — First-hour guide: setup, test, run, common workflows
  architecture/
    system-context.mmd           — System-level context diagram (Mermaid/D2)
    container-view.mmd           — Container-level view
    component-map.mmd            — Component relationships
    adr-0001-<title>.md          — Architecture Decision Records
  components/
    <component-name>/
      README.md                  — Responsibilities, owners, key modules
      interfaces.md              — APIs/events exposed and consumed
      data-model.md              — Domain entities, tables, types owned
      runtime.md                 — Deployment units, scaling, critical configs
      diagrams/
  flows/
    <flow-name>/
      README.md                  — Narrative: triggers, happy path, failure modes
      sequence.mmd               — Sequence diagram (visual index of the flow)
      state.mmd                  — State machine (lifecycle, transitions)
      contracts.md               — Interfaces involved, payload definitions
      runbook.md                 — When this breaks, what to do
  api/
    <service-or-domain>/
      openapi.yaml               — Published API contracts
      events.md                  — Event definitions, topics, payloads
  data/
    schemas/                     — Shared schemas (JSON Schema, protobuf, zod)
    lineage.md                   — Source of truth, derived-from relationships
    migrations.md                — Conceptual migration notes
  ops/
    deploy.md                    — Deploy procedures, environments, config
    observability.md             — Dashboards, traces, logs, SLOs
    alerts.md                    — What each alert means
    incidents/                   — Postmortems
  references/
    glossary.md                  — Domain terms, abbreviations
    conventions.md               — Naming, error formats, idempotency rules
    tooling.md                   — Internal scripts, make targets
```

Each layer serves a distinct purpose. `overview.md` is the front door — what the system does, what it depends on, where to start. `getting-started.md` is the developer's first hour — setup, test, run. `architecture/` holds global decisions (ADRs), structural diagrams that no single component owns, and a top-level map of domains and package layering. `components/` maps the pieces. `flows/` maps how the pieces relate. `api/` and `data/` capture contracts. `ops/` captures operational knowledge. `references/` holds the small artifacts that prevent recurring questions.

Two additions from practitioner experience deserve emphasis. First, **quality grading**: a document that grades each product domain and architectural layer, tracking gaps over time. This gives agents (and humans) a machine-readable signal about which areas of the codebase are mature and which need care — agents can adjust their confidence and verification effort accordingly. Second, **plans as first-class artifacts**: execution plans with progress and decision logs are checked into the repository alongside active work, completed work, and known technical debt. An agent picking up a task reads the plan, understands what has been done, what remains, and what decisions were made — without relying on external project management tools or human memory ([Harness Engineering](../SOURCES.md#harness-engineering)).

## Components: What the Pieces Are

A component is anything you can point at in both code and runtime — a service, a library with a clear boundary, a frontend app, a data pipeline job. Each component folder answers "what is this piece and where is it?" through a consistent set of files:

- **README.md** — responsibilities, owners, how to run, key modules and directories
- **interfaces.md** — APIs and events exposed and consumed, with links to `docs/api/`
- **data-model.md** — domain entities, tables, and types the component owns
- **runtime.md** — deployment units, scaling characteristics, critical configuration

The regularity matters. An agent encountering any component folder knows exactly what files to expect and what questions each file answers. No discovery phase, no guessing at conventions.

## Flows: How the Pieces Relate

A flow is an end-to-end behavior that crosses component boundaries — a user journey, a business process, an event-driven chain. Flows answer "how does X actually happen?" and are where architecture becomes operationally true.

Examples: "user signs up" (frontend, API, auth provider, database, email), "invoice generated" (cron, billing service, payments, ledger), "webhook ingestion" (edge, queue, processor, retries, dead letter queue), "search index update" (event, consumer, indexer, backfill).

Each flow folder contains:

- **README.md** — the narrative: trigger(s), happy-path steps, failure modes, retry and idempotency semantics, consistency model, links to involved components and contracts
- **sequence.mmd** — Mermaid sequence diagram showing actors and calls, serving as the visual index of the flow
- **state.mmd** — state machine diagram for flows with lifecycle (order states, subscription status, onboarding stages)
- **contracts.md** — the interfaces involved: endpoints, events, schemas, payload semantics, versioning
- **runbook.md** — when this breaks: how to replay messages, dedupe, find correlation IDs, backfill, with links to dashboards and log queries

Flows are particularly valuable in polyglot environments — a TypeScript frontend, Node edge services, and Python backends connected by HTTP and event-driven processing. The flow folder becomes the single durable artifact that spans languages and surfaces API contracts at boundaries, queue and event semantics, retry patterns, and ownership across services.

## Lightweight Rules

A small rule set keeps documentation consistent without requiring heavy governance:

- Every component gets a folder when it is deployed independently or imported by multiple other components.
- Every flow gets a folder when it spans more than one component or has non-trivial failure modes.
- Every flow diagram must link back to the components it touches and the contracts it uses.
- Flows use stable names (`invoice-generation`) rather than UI labels or ticket numbers.

These rules are deliberately mechanical — they exist to be enforced by machines, not negotiated by humans.

## Mechanical Enforcement

Structured documentation without mechanical enforcement is a graveyard on a schedule. The most important lesson from practitioners who have made cartography durable: **enforce it the same way you enforce code quality — with linters, CI, and automated remediation** ([Harness Engineering](../SOURCES.md#harness-engineering)).

**Linters and CI jobs** validate structural integrity on every commit:

- Every deployed service has a corresponding `docs/components/` folder
- Every flow README links to at least two components
- Diagram files (Mermaid, D2) parse correctly
- Cross-links between documentation files resolve
- Required sections exist in component and flow READMEs
- Quality grades are present and not stale

These checks run in CI alongside tests and type checks. A PR that adds a new service without a component folder fails the same way a PR without tests fails — the documentation structure is not optional, it is part of the build.

**Doc-gardening agents** handle the maintenance that humans reliably neglect. A recurring background agent scans for documentation that no longer reflects the real code behavior — a component README that lists modules that have been renamed, a flow diagram that references a service that has been decomposed, an API contract that omits recently added fields — and opens targeted fix-up PRs. Most of these are small, reviewable in under a minute, and eligible for automerge. The gardening agent turns documentation freshness from a periodic human discipline into a continuous automated process.

**The promotion pattern** closes the loop: when documentation proves insufficient — an agent misinterprets a flow, a review catches a recurring misunderstanding — the fix is not just a documentation update but a promotion into code. A vague convention becomes a lint rule. A naming pattern becomes a structural test. Documentation that fails to prevent mistakes gets replaced by mechanical constraints that make the mistakes impossible.

The result is a knowledge base that is not merely written but *maintained* — mechanically verified, continuously gardened, and progressively hardened into enforceable rules. Without this enforcement layer, cartography is a one-time investment that decays; with it, the documentation compounds in accuracy and coverage over time.

## Why This Matters for Agents

Code indexing tells an agent *where* things are. Cartography tells it *what they mean*. An agent asked to "add retry logic to the webhook ingestion pipeline" benefits enormously from a `docs/flows/webhook-ingestion/` folder that explains the current retry semantics, links to the relevant components, defines the contracts involved, and provides a runbook for when retries fail. Without cartography, the agent must reconstruct this understanding from scattered code comments, variable names, and test fixtures — a slow, error-prone process that degrades with system complexity.

The documentation architecture also provides a natural specification surface. When an agent is asked to implement a new flow, the expected output is not just code but a populated `docs/flows/<flow-name>/` folder — README, sequence diagram, contracts, runbook — that proves the agent understood the system well enough to document it.

## Implements

- [Agent-Native Environment](../principles/agent-native-environment.md) — structured documentation as part of the environment agents need to reason about systems
- [Compound Knowledge](../principles/compound-knowledge.md) — cartography artifacts accumulate understanding that persists across sessions and team changes

## See Also

- [Codebase Indexing](codebase-indexing.md) — the search layer that cartography complements: indexing finds code, cartography explains systems
- [Progressive Disclosure](progressive-disclosure.md) — layered context management; cartography provides the layers that progressive disclosure navigates
- [Filesystem as Memory](filesystem-as-memory.md) — the underlying substrate; cartography is a specific application of filesystem-as-memory to system documentation
- [Pyramid Summaries](pyramid-summaries.md) — the compression technique; cartography's overview → component → flow hierarchy is a pyramid structure
- [Specification Discipline](specification-discipline.md) — flow folders provide the starting location and done criteria that good specifications require
