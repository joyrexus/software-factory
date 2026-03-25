# Living Spec

*The canonical artifact format for the spec-driven agentic SDLC.*

## Contents

- [What a Living Spec Is](#what-a-living-spec-is) — Markdown document, versioned, co-authored by human and agent
- [YAML Frontmatter](#yaml-frontmatter) — the machine-parseable subset for the graph compiler
- [Section Anatomy](#section-anatomy) — eight sections, who writes each one, when, and what it contains
- [Protected Markers](#protected-markers) — preserving human-authored content through agent editing
- [The Lifecycle](#the-lifecycle) — authoring → checkpoint → implementation → verification
- [Complete Example](#complete-example) — annotated canonical example
- [See Also](#see-also)

---

## What a Living Spec Is

A living spec is a single Markdown document that defines a feature's behavioral contract, tracks implementation progress, and records architectural decisions — all in one versioned artifact. It is written partly by the human, partly drafted by a coding agent (via the `spec-seed` skill), and updated by the agent as implementation proceeds.

At any given moment, a living spec reads like a cross between an RFC, an ADR, and a project tracker — which is more or less exactly what it is.

The term "living" distinguishes this from a static spec that freezes at authoring time. A living spec:

- Starts with human-authored intent (goal and constraints)
- Gets seeded with behavioral detail by the agent (output specification, success criteria, invariants, scenarios, tasks)
- Is reviewed and edited by the human at a checkpoint before implementation begins
- Evolves during implementation: tasks get checked off, decisions get recorded
- Is verified against its own success criteria before the agent surfaces work for human review

---

## YAML Frontmatter

The first block of a living spec is YAML frontmatter — the machine-parseable subset that feeds the graph compiler. Frontmatter is transparent to the human author; it reads like a metadata header.

```yaml
---
feature: create_invoice
domain: invoicing
inputs: [authenticated_user, invoice_items[]]
effects: [invoice_persisted, invoice_created_event]
---
```

These four fields are what the `compile-spec-graph.ts` script reads to build the spec graph: nodes (`feature`, `domain`) and edges (`inputs` → upstream dependencies, `effects` → downstream consumers). Everything below the frontmatter block is the living spec body — human-readable and agent-navigable.

---

## Section Anatomy

A complete living spec contains eight sections. The first two are human-authored and protected; the remainder are agent-seeded and agent-maintained.

### Goal (human-authored, protected)

The outcome the feature must produce, stated without prescribing a solution.

- Good: "Enable the billing team to create invoices without manual data-entry errors"
- Bad: "Add a form with validation and a submit button"

Goals name what changes for users; solutions name implementation details. A goal framed this way remains valid as the implementation evolves; a solution framing becomes stale the moment a different approach is chosen. The bad/good contrast is elaborated in [Specification Discipline](specification-discipline.md).

### Constraints (human-authored, protected)

What the agent must not change: frozen modules, existing API contracts, hard limits, and non-negotiable invariants.

```markdown
## Constraints
- Frozen: `invoice_schema_v2`, payment-service HTTP API (v3)
- No new database tables
- Must integrate with existing user auth middleware
```

Constraints stated before the agent starts prevent scope drift. Constraints discovered mid-implementation require a rework — they are the leading cause of implementation backtrack. Both Goal and Constraints are wrapped in protected markers (see below).

### Output Specification (agent-seeded)

What the feature must produce, as a deliverable list. Not the implementation approach — the artifacts.

```markdown
## Output Specification
- POST /api/invoices endpoint accepting authenticated user + line items
- Middleware validation for invariants (non-empty items, total check)
- invoice_created_event emitted on successful persist
- Unit tests covering happy path + 3 error cases
```

The Output Specification is the narrative description of what to build. The Success Criteria are the machine-checkable assertions that prove it is built correctly. Both are needed; they play different roles.

### Success Criteria (agent-seeded)

Measurable assertions that define done. Each criterion must be checkable — by running a command, observing a metric, or reading an artifact. Vague criteria ("works correctly") are not success criteria.

```markdown
## Success Criteria
- [ ] Invoice creation error rate < 2% over first 30 days post-deploy
- [ ] End-to-end latency < 500ms at p95 under normal load
- [ ] No breaking changes to existing invoice endpoints
```

The Verifier agent runs against these before surfacing work for human review. They double as CI acceptance criteria and the done-definition for the spec gate in the [GitHub workflow](../meta/agent-native/sdlc/github-workflow.md). A measurable success criterion is worth a hundred vague requirements.

### Invariants (agent-seeded)

Non-negotiable correctness conditions derived from constraint elicitation and codebase analysis. These are the assertions the system must maintain regardless of implementation approach.

```markdown
## Invariants
- `total == sum(item.amount for item in invoice_items)`
- `invoice_items` is non-empty
```

### Scenarios (agent-seeded)

Behavioral scenarios in Given/When/Then format. These are the machine-executable acceptance tests that feed the CI verification gate — the `spec-verify.yml` workflow runs these against every spec touched by a PR.

```markdown
## Scenarios
- Given: authenticated user with 3 valid line items
  When: POST /api/invoices
  Then: 201 + invoice record persisted + invoice_created_event on queue

- Given: authenticated user with empty line items
  When: POST /api/invoices
  Then: 422 Unprocessable Entity
```

Scenarios are the closest thing to BDD in this framework. As [Jain](../SOURCES.md#how-to-kill-the-code-review) observes, the calculation that made structured behavioral specifications feel like extra work inverts in an agentic workflow: the spec IS the work. The human writes the scenario; the agent implements the code; the verification gate checks satisfaction.

### Tasks (agent-seeded, agent-maintained)

The live implementation tracker. Seeded during spec authoring; checked off as work proceeds; annotated with agent assignment.

```markdown
## Tasks
- [x] Surface context — behavioral neighbors, adjacent invariants (complete)
- [x] Elicit constraints — scope boundary confirmed: create only, not void (complete)
- [ ] Seed spec draft — Spec Agent (in progress)
- [ ] Human checkpoint — review and edit
- [ ] Implement POST handler — Impl Agent
- [ ] Verify against success criteria — Verifier Agent
```

The agent updates this section throughout implementation. Before surfacing any work for human review, the agent verifies completed items against the Success Criteria — Tasks tracks what has been done; Success Criteria confirms that what has been done is correct.

### Decision Log (agent-appended)

An append-only log of architectural decisions, datestamped as they are made.

```markdown
## Decision Log
- 2026-03-15: Using invoice_schema_v2 without extension — frozen per billing-service contract; no new columns needed
- 2026-03-16: Scope boundary: invoice creation only; void_invoice is a separate spec
```

Entries are never edited or deleted — only appended. An undocumented decision is an ambiguity waiting to cause a merge conflict. The graph compiler ignores the Decision Log; agents navigating a new domain read it for the architectural context the behavioral contract intentionally omits.

---

## Protected Markers

The Goal and Constraints sections are wrapped in `<!-- BEGIN USER-SPECIFIED -->` / `<!-- END USER-SPECIFIED -->` markers. These signal to agents that the enclosed content is human-defined and should not be overwritten when the spec self-updates.

```markdown
<!-- BEGIN USER-SPECIFIED -->
## Goal
Enable billing team to create invoices without manual data-entry errors.

## Constraints
- Frozen: `invoice_schema_v2`, payment-service HTTP API (v3)
- No new database tables
<!-- END USER-SPECIFIED -->
```

Without markers, an agent updating a spec might rationalize revising the constraints to simplify its implementation path. The markers are not a technical lock — they are a semantic signal that the enclosed content has a different authorship model than the rest of the document.

Use protected markers only for content the human owns unconditionally: Goal and Constraints. The remaining sections are agent territory — the human may edit them during the checkpoint, but they are not protected by default.

---

## The Lifecycle

A living spec evolves through four phases:

**Authoring** — Human writes Goal and Constraints (inside protected markers). The `spec-seed` skill runs codebase analysis and seeds Output Specification, Success Criteria, Invariants, Scenarios, and an initial Tasks list. This is Stage 3 of the [spec authoring workflow](../meta/agent-native/sdlc/spec-authoring.md).

**Human checkpoint** — Human reviews the seeded draft, edits any section, and approves before implementation begins. The self-check heuristic from [Specification Discipline](specification-discipline.md) guides this review: Can I identify where the agent begins reading? What artifact proves completion? Are the constraints explicit enough to produce deterministic work?

**Implementation** — Agent works through the Tasks list, checking off items as they complete. The Decision Log grows as architectural choices are made. The agent does not surface work for human review until it has verified completed items against the Success Criteria.

**Graph integration check** — The spec passes through the automated graph compiler gate (Stage 4 of the authoring workflow). Orphaned nodes, circular dependencies, and missing event consumers are caught before the spec PR merges. A spec that passes is structurally coherent with the rest of the graph.

---

## Complete Example

```markdown
---
feature: create_invoice
domain: invoicing
inputs: [authenticated_user, invoice_items[]]
effects: [invoice_persisted, invoice_created_event]
---

# Create Invoice — Living Spec

<!-- BEGIN USER-SPECIFIED -->
## Goal
Enable billing team to create invoices without manual data-entry errors.

## Constraints
- Frozen: `invoice_schema_v2`, payment-service HTTP API (v3)
- No new database tables
<!-- END USER-SPECIFIED -->

## Output Specification
- POST /api/invoices endpoint accepting authenticated user + line items
- Middleware validation for invariants (non-empty items, total check)
- invoice_created_event emitted on successful persist
- Unit tests: happy path + empty items + schema violation

## Success Criteria
- [ ] Invoice creation error rate < 2% over first 30 days post-deploy
- [ ] End-to-end latency < 500ms at p95 under normal load
- [ ] No breaking changes to existing invoice endpoints

## Invariants
- `total == sum(item.amount for item in invoice_items)`
- `invoice_items` is non-empty

## Scenarios
- Given: authenticated user with 3 valid line items
  When: POST /api/invoices
  Then: 201 + invoice record persisted + invoice_created_event on queue

- Given: authenticated user with empty line items
  When: POST /api/invoices
  Then: 422 Unprocessable Entity

## Tasks
- [x] Surface context — behavioral neighbors, adjacent invariants (complete)
- [x] Elicit constraints — scope boundary confirmed: create only, not void (complete)
- [ ] Seed spec draft — Spec Agent (in progress)
- [ ] Human checkpoint — review and edit
- [ ] Implement POST handler — Impl Agent
- [ ] Verify against success criteria — Verifier Agent

## Decision Log
- 2026-03-15: Using invoice_schema_v2 without extension — frozen per billing-service contract; no new columns needed
- 2026-03-16: Scope boundary: invoice creation only; void_invoice is a separate spec
```

---

## See Also

- [Specification Discipline](specification-discipline.md) — the self-check heuristic for reviewing a living spec before implementation begins, and the bad/good contrast for what good requirements actually look like
- [Spec Authoring Workflow](../meta/agent-native/sdlc/spec-authoring.md) — the four-stage process that produces the living spec
- [Pilot Guide](../meta/agent-native/sdlc/pilot-guide.md) — the repo structure that hosts living specs and the graph compiler that reads their frontmatter
- [Spec-Driven Architecture](../meta/agent-native/sdlc/spec-driven-architecture.md) — the four-layer architecture the living spec feeds into
- [Scenarios Not Tests](scenarios-not-tests.md) — the validation philosophy behind the Scenarios and Success Criteria sections
