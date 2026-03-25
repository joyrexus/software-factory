# Spec Authoring Workflow

*How intent becomes an executable spec grounded in system context.*

---

Every document in this directory assumes a spec exists. The [pilot guide](pilot-guide.md) describes the spec format; [spec-driven-architecture.md](spec-driven-architecture.md) describes the architecture it feeds into; [github-workflow.md](github-workflow.md) describes the PR gate that enforces it. None of them describe how you get from "we need to add invoice creation" to a completed `create_invoice.yaml` that is grounded, complete, and ready to compile.

That gap is what this document addresses. Spec authoring is not a preliminary step before engineering begins — it is engineering, at the level of abstraction where the cost of error is lowest and the leverage over subsequent work is highest. A spec authored without discipline produces the same failure modes as code written without tests: the errors are discovered later, at greater cost, after more work has been built on top of them.

---

## Contents

- [The Expanded Progression](#the-expanded-progression) — how the full intent→verify pipeline expands beyond the common summary
- [Why Authoring Is the Critical Stage](#why-authoring-is-the-critical-stage) — the temporal argument for why specs are the cheapest place to be wrong
- [The Four Stages](#the-four-stages) — surface context → elicit constraints → draft → graph integration check
  - [Stage 3: The Spec Seed Skill](#stage-3-draft-and-review-the-spec) — human writes goal + constraints; agent seeds the rest
- [What This Produces](#what-this-produces) — the spec PR artifact the rest of the framework operates on
- [See Also](#see-also)

## The Expanded Progression

The framework's development model is typically summarized as:

```
Agentic SDLC:   intent → plan → implement → verify
```

That compression hides where most of the intellectual work happens. The full progression is:

```
intent
  → surface system context
  → elicit constraints and scope
  → draft spec
  → graph integration check
  → compile into graph
  → plan implementation
  → implement
  → verify
```

The first four steps — from intent through graph integration check — constitute the spec authoring workflow. Everything from "compile into graph" onward is documented elsewhere in this directory. This document covers the four upstream steps.

---

## Why Authoring Is the Critical Stage

Two claims from [clarity-constraint.md](clarity-constraint.md) converge here.

**Claim 2** (the temporal argument) establishes that the spec is the cheapest place to be wrong. A failure mode identified during authoring costs a line edit. The same failure mode discovered during implementation costs a debugging session; discovered in production, it costs an incident. Authoring discipline is upstream leverage.

**Claim 6** (the two-component spec) establishes that a complete spec requires two distinct inputs: human judgment about what success means and which constraints are non-negotiable, and system context about existing patterns, dependencies, and failure modes. The authoring workflow is the process of assembling both inputs before drafting anything.

There is also a structural tension the authoring stage must resolve. The [Executable Spec](../../../GLOSSARY.md#executable-spec) is deliberately implementation-agnostic — a behavioral contract, not an architectural blueprint. That is the right design for the reasons stated in `spec-driven-architecture.md`. But an agent implementing a new spec in a new domain needs architectural context the spec intentionally omits. The behavioral contract is the agent's *goal*; architectural context is the agent's *map*. Both need to be in place before implementation begins. For existing domains, the [Behavioral Index](../../../GLOSSARY.md#behavioral-index) provides the map by traversing from spec nodes to implementing code. For new domains, that index is empty — and the authoring workflow is where the map gets started.

---

## The Four Stages

### Stage 1: Surface system context

Before drafting anything, query what the system already knows. For domains already represented in the [Spec Graph](../../../GLOSSARY.md#spec-graph), the [Behavioral Index](../../../GLOSSARY.md#behavioral-index) is the right artifact to query:

- What spec nodes already exist in this domain?
- What events would this new behavior consume or emit?
- What invariants do adjacent specs impose on shared state?
- What existing tests provide coverage this spec should not duplicate?

For a new domain with no index entries, [codebase cartography](../../../techniques/codebase-cartography.md) artifacts — `docs/components/`, `docs/flows/`, `docs/architecture/` — fill this role. If those don't exist yet, the investigation itself produces the first cartographic artifact. Either way, this stage is agent-driven: a human writing a spec from memory will miss the system context that makes requirements actually grounded.

The output of Stage 1 is a context summary: the existing behavioral neighbors, the relevant invariants, the events this spec participates in, and any known failure modes in this domain.

### Stage 2: Elicit constraints and scope

With system context in hand, the human and agent work through a structured constraint elicitation — not open-ended chat, but a specific checklist:

- **Invariants:** What are the non-negotiable correctness conditions? (e.g., `total == sum(item.amount for item in items)`)
- **Dependencies:** What external systems does this behavior depend on?
- **Events:** What must this behavior emit for downstream consumers?
- **Failure modes:** What are the known ways this can fail, and what should happen in each case?
- **Frozen contracts:** What must not change — API surfaces, event schemas, existing caller expectations?
- **Scope boundaries:** What is explicitly *not* in this spec? Where does this behavior end and an adjacent behavior begin?

The scope boundary question is often the hardest. "Create invoice" vs. "apply discount to invoice" vs. "validate invoice before submission" are contested boundary decisions that different stakeholders draw differently. Surfacing and resolving these during elicitation — not during implementation — is exactly the leverage Claim 2 describes.

The output of Stage 2 is a constraint checklist that drives both the spec draft and the `decisions:` section.

### Stage 3: Draft and review the spec

Stage 3 is where the spec-authoring plugin's `spec-seed` skill takes over. It takes the constraint checklist from Stage 2 as input and produces a complete YAML draft through codebase analysis — but it only seeds what the agent can derive. The human must first provide two things.

**What the human writes first.** Before the `spec-seed` skill runs, the human authors two sections of the spec directly, guided by four rules:

- **Name the goal, not the solution.** "Reduce onboarding drop-off by 20%" is a goal. "Add a progress bar" is a premature solution. Goals stay valid as the implementation evolves; solutions don't.
- **State constraints explicitly.** Frozen modules, existing API contracts, hard limits — list them before any agent starts. Constraints discovered mid-implementation are the leading cause of scope drift.
- **Define done measurably.** A measurable success criterion is worth a hundred vague requirements. "Invoice creation error rate < 2% in 30 days" is done. "Works correctly" is not.
- **Record decisions as they're made.** An undocumented decision is an ambiguity waiting to cause a merge conflict. The `decisions:` section is populated throughout authoring and implementation — not written after the fact.

**What the agent seeds.** With `goal` and `constraints` in place, the `spec-seed` skill runs codebase analysis against the Stage 1 context summary and seeds the remaining fields: `feature`, `domain`, `inputs`, `invariants`, `effects`, `scenarios`, `success_criteria`, and an initial `tasks` checklist. The skill uses the existing behavioral neighbors, known failure modes, and event patterns surfaced in Stage 1 to ground each field in actual system state rather than assumption.

**The human checkpoint.** The agent's seed is a draft, not a final artifact. The human reviews it against the self-check heuristic from [Specification Discipline](../../../techniques/specification-discipline.md):

1. Can I identify where the agent begins reading?
2. What artifact proves completion?
3. Are the constraints explicit enough to produce deterministic work?

If any answer is "no," the spec is not ready to compile. Common failure modes: invariants that reference undefined state, scenarios that don't cover the failure modes surfaced in Stage 1, scope boundaries stated verbally but not encoded in the spec. The human edits before any implementation begins.

**The living document.** After the human checkpoint, the spec doesn't freeze — it tracks. As the agent works, it updates the `tasks` section (checking off completed items) and populates `decisions` as architectural choices are made. Before surfacing any work for human review, the agent verifies completed work against `success_criteria`. At any given moment the spec reads like a cross between an RFC, an ADR, and a project tracker — which is more or less exactly what it is.

### Stage 4: Graph integration check

Before the spec PR merges, the graph compiler runs against the draft. This is an automated gate, not a human review step. It verifies:

- No orphaned nodes (spec nodes that declare dependencies with no corresponding spec in the graph)
- No circular dependencies
- No missing edges to declared event consumers (if the spec emits `invoice_created_event`, there must be a spec node that handles it, or the gap must be explicitly acknowledged)

A spec that passes the integration check is structurally coherent with the rest of the graph. It can now be merged, compiled, and used as the input to implementation planning. Everything downstream — the behavioral index update, the CI verification gate, the implementation PR — proceeds from this point.

---

## What This Produces

The output of a completed spec authoring workflow is a spec PR containing:

- A [Living Spec](../../../techniques/living-spec.md) Markdown document with YAML frontmatter, including:
  - Human-authored Goal and Constraints (inside protected markers)
  - Agent-seeded Output Specification, Success Criteria, Invariants, Scenarios, and Tasks
- A Decision Log populated throughout authoring (not written after the fact)
- A graph integration check that confirms structural coherence

The full section anatomy — who writes what, when, and the protected markers pattern — is documented in [Living Spec](../../../techniques/living-spec.md).

That PR is the artifact the rest of the framework operates on. The pilot guide's CI pipeline, the GitHub workflow's spec gate, and the adoption roadmap's Stage 2 criteria all assume this artifact exists and is complete. Spec authoring is the stage that produces it.

---

## See Also

- [Living Spec](../../../techniques/living-spec.md) — the canonical artifact format this workflow produces
- [clarity-constraint.md](clarity-constraint.md) — the argumentative foundation for why specs require both human judgment and system context
- [spec-driven-architecture.md](spec-driven-architecture.md) — the four-layer architecture the authored spec feeds into
- [pilot-guide.md](pilot-guide.md) — the repo structure that hosts living specs and the graph compiler that reads their frontmatter
- [github-workflow.md](github-workflow.md) — the spec PR gate this workflow feeds into
- [Specification Discipline](../../../techniques/specification-discipline.md) — quality criteria and the self-check heuristic
- [Codebase Cartography](../../../techniques/codebase-cartography.md) — the technique for producing the architectural map Stage 1 queries
- [Behavioral Index](../../../GLOSSARY.md#behavioral-index) — the artifact Stage 1 queries for existing-domain context
