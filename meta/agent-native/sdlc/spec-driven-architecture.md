# Spec-Driven Architecture

*Core concepts and the behavioral control plane.*

## Overview

AI-assisted development dramatically increases the rate at which code can be produced. Traditional SDLC processes — code review, unit tests, and manual QA — were not designed for this velocity.

The emerging response is a shift from **code-centric development** to **behavior-centric development**: system behavior, expressed as structured specifications, becomes the primary artifact governing development. Code becomes the implementation layer; behavior becomes the control plane.

This architectural approach instantiates two principles from the broader software factory framework:

- **[The Seed](../../../principles/seed.md)** — specifications are the starting point whose quality determines the output ceiling
- **[Validation](../../../principles/validation.md)** — behavioral verification replaces code-level testing as the primary proof of correctness

In this model:
- **Specs** define intended behavior
- **Code** implements that behavior
- **CI** verifies behavioral correctness
- **AI agents** operate within these guardrails

---

## The Four-Layer Architecture

### 1. Executable Specifications (Intent Layer)

Features begin with structured behavioral specifications rather than implementation. These specifications act as machine-readable behavioral contracts — a concrete application of [Specification Discipline](../../../techniques/specification-discipline.md).

**Example:**

```yaml
id: create_invoice

inputs:
  - line_items
  - customer_id

preconditions:
  - user_authenticated

postconditions:
  - invoice_persisted
  - total_equals_sum_line_items

emits:
  - invoice.created
```

This format directly answers the [self-check heuristic](../../../techniques/specification-discipline.md): the agent knows where to start (the spec), what to implement (the postconditions), and what proves completion (the emitted events and persisted state).

**Benefits:**
- Clearer feature intent
- Improved cross-team communication
- Deterministic verification of behavior

---

### 2. Spec Graph (System Behavior Model)

Individual specs connect into domain-level graphs describing how behaviors compose.

**Example:**

```
create_invoice → capture_payment → ledger_entry
```

This graph becomes the behavioral model of the product — a machine-readable form of the structured documentation that [Codebase Cartography](../../../techniques/codebase-cartography.md) advocates. It represents:

- Workflow transitions
- Event propagation
- State dependencies
- System invariants

**Benefits:**
- Architecture visibility
- Dependency awareness
- Safe refactoring

---

### 3. Behavioral Index (Code Navigation Layer)

The spec graph becomes a behavioral index over the codebase — mapping specifications to the code that implements them.

**Example mapping:**

```yaml
node: create_invoice

code:
  - src/invoice/service.ts

tests:
  - tests/invoice.spec.ts
```

AI agents and developers navigate the system through behavior:

```
behavior → implementation → verification
```

instead of:

```
file search → guess intent
```

This extends the capabilities of [Codebase Indexing](../../../techniques/codebase-indexing.md) from "where is this symbol?" to "what code implements this behavior?"

**Benefits:**
- Reliable AI code navigation
- Improved developer onboarding
- Faster impact analysis

---

### 4. Behavioral CI Guardrails (Verification Layer)

CI pipelines use the spec graph to determine what verification should run.

A change to `create_invoice` triggers graph traversal, identifying dependent behaviors:
- `capture_payment`
- `ledger_entry`

CI then runs targeted verification for those behaviors.

**Verification approaches include:**

| Approach | Focus |
|---|---|
| **Spec verification** | Ensures implementation satisfies the specification (e.g., [Aviator Verify](../../../SOURCES.md)) |
| **Scenario exploration** | Explores realistic workflows to discover failures (e.g., [Ranger](../../../SOURCES.md)) |

Together they verify both **intended behavior** and **emergent system behavior** — applying the principle of [independent validation](../../../principles/validation.md) where the verifier is structurally separate from the implementer.

**Benefits:**
- Behavior-aware CI
- Targeted testing
- Safer continuous deployment

---

## The Behavioral Control Plane

The four layers above compose into what practitioners describe as a **behavioral control plane** — a governance layer above the code that separates three concerns traditional SDLC mixes together.

### Implementation Plane

Where code lives: services, APIs, jobs, UI logic, databases. This is the mechanical layer — the part AI agents are increasingly capable of generating.

### Behavioral Control Plane

Where system behavior is modeled and governed. The combination of the spec graph and behavioral index provides:

- **A behavior model** — the graph of system behaviors and their dependencies
- **A navigation layer** — the mapping from behavior to implementation
- **An impact analysis engine** — graph traversal that determines what a change affects

This layer enables AI agents and CI systems to reason about the system structurally rather than heuristically. An agent asked to modify invoicing can traverse the graph to understand downstream effects on payments and ledger entries — knowledge that would otherwise require implicit architectural understanding.

### Intent and Verification Layers

The intent layer (executable specs) defines what the system *should* do. The verification layer ensures it *does*. Together they close the loop: intent → implementation → verification → feedback.

---

## The Resulting Development Model

**Traditional SDLC:**
```
requirements → code → tests → deploy
```

**Spec-driven SDLC:**
```
spec → plan → implement → behavioral verification → deploy
```

Behavior becomes the control plane of development. Code becomes the implementation layer. This reframing is particularly powerful for AI agents, which operate most effectively when they have explicit intent, navigable system structure, and deterministic guardrails.

---

## How AI Agents Fit Into This Model

```
read spec
  → plan implementation
  → navigate code via behavioral index
  → generate implementation
  → validate via CI guardrails
```

The spec-driven architecture provides exactly the three things agents need: a precise [seed](../../../principles/seed.md) (the spec), an objective [validation](../../../principles/validation.md) mechanism (behavioral CI), and a [feedback loop](../../../principles/feedback-loop.md) (verification results driving iteration).

---

## Organizational Impact

Adopting this model shifts engineering conversations from "what code should we change?" to "what behaviors are affected?" It introduces:

- Clearer system modeling
- Safer refactoring
- AI-compatible architecture
- More reliable CI pipelines

The shift parallels the broader pattern described in the [Agent Readiness Model](../maturity-model.md): organizations that invest in structured context and automated verification — rather than relying on human judgment at every step — create environments where agents can operate effectively.

---

## See Also

- [Specification Discipline](../../../techniques/specification-discipline.md) — the self-check heuristic that executable specs implement
- [Codebase Cartography](../../../techniques/codebase-cartography.md) — structured documentation architecture that the spec graph extends
- [Codebase Indexing](../../../techniques/codebase-indexing.md) — code search that the behavioral index builds on
- [Scenarios Not Tests](../../../techniques/scenarios-not-tests.md) — holdout-set validation as an alternative to spec verification
- [Risk-Tiered Automation](../../../techniques/risk-tiered-automation.md) — graduated autonomy levels for verification gates
- [Architecture Reference](architecture-reference.md) — visual diagram of the full platform
- [GitHub Workflow](github-workflow.md) — how this architecture operationalizes in a GitHub-centric process
- [Adoption Roadmap](adoption-roadmap.md) — staged path from test-centric to spec-driven development
