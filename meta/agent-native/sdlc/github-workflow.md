# Spec-Gated GitHub Workflow

*How a SaaS organization operationalizes spec-driven development within a GitHub-centric process.*

## Contents

- [Workflow Diagram](#workflow-diagram) — end-to-end ASCII diagram from spec PR through deployment and runtime feedback
- [How This Workflow Operates](#how-this-workflow-operates) — step-by-step walkthrough of the eight-step spec-gated process
- [Why This Model Works Well With AI Coding](#why-this-model-works-well-with-ai-coding) — how spec-driven guardrails scale with AI-generated PR volume
- [See Also](#see-also)

## Workflow Diagram

```
             Product Manager / Architect
                        │
                        ▼
               ┌──────────────────────┐
               │  Executable Spec PR  │
               │  (Intent Definition) │
               └──────────┬───────────┘
                          │
                          ▼
              ┌────────────────────────┐
              │     Spec Compiler      │
              │  Build / Update Spec   │
              │        Graph           │
              └──────────┬─────────────┘
                         │
                         ▼
              ┌────────────────────────┐
              │    Behavioral Index    │
              │ Spec ↔ Code Mapping    │
              └──────────┬─────────────┘
                         │
                         ▼
                 ┌─────────────────┐
                 │ Planning Engine │
                 │ Spec → Task DAG │
                 └─────────┬───────┘
                           │
                           ▼
                  ┌──────────────────┐
                  │ Implementation   │
                  │ AI Coding Agent  │
                  │ or Developer     │
                  └─────────┬────────┘
                            │
                            ▼
                     Code Change PR
                            │
                            ▼
         ┌─────────────────────────────────┐
         │          CI PIPELINE            │
         │                                 │
         │ 1. Spec Verification            │
         │ 2. Impact Analysis              │
         │    (Spec Graph Traversal)       │
         │ 3. Scenario Exploration         │
         │ 4. Regression + Contract Tests  │
         └───────────────┬─────────────────┘
                         │
                         ▼
                   Merge Gate
                         │
                         ▼
                   Deployment
                         │
                         ▼
                 Runtime Telemetry
                         │
                         ▼
                 Spec Graph Updates
```

---

## How This Workflow Operates

### Step 1 — Feature Development Begins With a Spec PR

Instead of starting with code, the team proposes a spec file — applying [specification discipline](../../../techniques/specification-discipline.md) to produce a machine-readable intent artifact.

```
specs/invoicing/create_invoice.yaml
```

**Example intent artifact:**

```yaml
feature: create_invoice

inputs:
  - authenticated_user
  - invoice_items

invariants:
  - total == sum(invoice_items)

effects:
  - invoice_persisted
  - invoice_created_event
```

This single file serves as instruction to agents, acceptance criteria for CI, and behavioral documentation for engineers.

---

### Step 2 — CI Compiles the Spec Into the Spec Graph

The [spec graph](../../../GLOSSARY.md#spec-graph) is a structured model of system behavior.

**Example node:**

```
create_invoice
  ├ requires: authenticated_user
  ├ produces: invoice_created_event
  └ affects: ledger
```

Edges represent behavior dependency, event propagation, and state transition. This graph becomes the **behavioral model of the product** — a machine-readable counterpart to the flow documentation described in [Codebase Cartography](../../../techniques/codebase-cartography.md).

---

### Step 3 — Behavioral Indexing Connects Specs to Code

The system records mappings from behaviors to their implementations:

```
create_invoice
  → InvoiceService.create()
  → POST /api/invoices
  → invoice_repository.save()
```

This index enables agent navigation, impact analysis, and targeted verification — extending [code indexing](../../../techniques/codebase-indexing.md) into behavioral navigation.

---

### Step 4 — Planning Layer Generates an Implementation Plan

The spec converts into a task graph that drives AI coding agents and developer implementation tasks:

```
create_invoice
 ├ validate request
 ├ compute totals
 ├ persist invoice
 ├ emit event
 └ send notification
```

---

### Step 5 — Code Is Implemented

Implementation may come from developers, AI coding agents, or automated refactoring systems. All code changes happen through normal **GitHub pull requests**.

---

### Step 6 — CI Runs Behavior-Aware Verification

Instead of running the entire test suite, the spec graph determines what must be verified. This is a form of [risk-tiered automation](../../../techniques/risk-tiered-automation.md) — verification effort scales with behavioral impact.

#### A. Spec Verification

Checks whether the implementation satisfies the intent specification. Spec verification tools (e.g., Aviator Verify) validate constraints, API contracts, policy enforcement, and state invariants.

#### B. Impact Analysis

The spec graph determines which behaviors may be affected:

```
create_invoice change
     ↓ affects
invoice_created_event
     ↓ affects
billing pipeline
```

Only impacted tests are prioritized.

#### C. Scenario Exploration

Explores emergent runtime behavior. Scenario exploration tools (e.g., Ranger) detect unexpected UI states, race conditions, integration failures, and edge cases. This complements deterministic spec verification — together they address both intended behavior and [emergent behavior](../../../techniques/scenarios-not-tests.md).

---

### Step 7 — CI Merge Gates Enforce Behavioral Integrity

A PR can merge only if:

```
✓ spec constraints satisfied
✓ affected behaviors verified
✓ scenario exploration passes
```

This turns CI into a **behavioral guardrail system** — implementing the [validation](../../../principles/validation.md) principle that the entity verifying must be independent of the entity implementing.

---

### Step 8 — Runtime Telemetry Feeds Back Into the Spec Graph

Production signals update the behavioral model — closing the [feedback loop](../../../principles/feedback-loop.md):

- New runtime state → new spec nodes
- Unexpected workflow path → new verification scenarios
- Rare edge case → new constraints

This creates a continuous learning loop where the spec graph becomes more accurate over time.

---

## Why This Model Works Well With AI Coding

AI coding increases PR volume, code generation speed, and the surface area of changes. The spec-driven workflow introduces guardrails that scale with velocity:

| Problem | Solution |
|---|---|
| Spec drift | Spec defines intent; verification enforces it |
| Silent regressions | Graph traversal identifies dependencies |
| Review overload | CI enforces correctness; humans review architecture |

This addresses the same review bottleneck that [Automated Review at Scale](../practices/README.md#automated-review-at-scale) identifies — behavioral CI reduces the volume of issues that require human judgment.

---

## See Also

- [Spec-Driven Architecture](spec-driven-architecture.md) — core concepts and the behavioral control plane
- [Architecture Reference](architecture-reference.md) — visual diagram of the full platform
- [Pilot Guide](pilot-guide.md) — concrete repo structure and 90-day pilot implementing this workflow
- [Specification Discipline](../../../techniques/specification-discipline.md) — the self-check heuristic for writing agent-ready specs
- [Scenarios Not Tests](../../../techniques/scenarios-not-tests.md) — holdout-set validation complementing spec verification
- [Risk-Tiered Automation](../../../techniques/risk-tiered-automation.md) — graduated verification levels
- [Linters as Architectural Guardrails](../practices/README.md#linters-as-architectural-guardrails) — spec verification as a behavioral linter
