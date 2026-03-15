# Architecture Reference

*Visual reference for the spec-driven agentic SDLC platform.*

## Platform Architecture

The diagram below shows the complete closed-loop architecture, from product intent through implementation and verification back to spec graph evolution.

```
                      ┌───────────────────────────────┐
                      │         PRODUCT TEAMS         │
                      │  PMs • Engineers • Architects │
                      └───────────────┬───────────────┘
                                      │
                                      ▼
                      ┌─────────────────────────────┐
                      │        INTENT LAYER         │
                      │                             │
                      │ Executable Specifications   │
                      │ Domain Invariants           │
                      │ Behavioral Contracts        │
                      │ Compliance Policies         │
                      └───────────────┬─────────────┘
                                      │
                                      ▼
                       ┌───────────────────────────┐
                       │        SPEC GRAPH         │
                       │                           │
                       │ Nodes: behaviors          │
                       │ Edges: domain flows       │
                       │ Event & state relations   │
                       └───────────────┬───────────┘
                                       │
                     ┌─────────────────┴─────────────────┐
                     ▼                                   ▼
           ┌───────────────────┐               ┌──────────────────┐
           │ Behavioral Index  │               │ Planning Engine  │
           │                   │               │                  │
           │ Spec ↔ Code map   │               │ Spec → Task DAG  │
           │ Spec ↔ Runtime    │               │ Feature plans    │
           └─────────┬─────────┘               └─────────┬────────┘
                     │                                   │
                     ▼                                   ▼
            ┌─────────────────────┐            ┌─────────────────────┐
            │  AI Coding Agents   │            │   Human Engineers   │
            │                     │            │                     │
            │ implement features  │            │ review + refine     │
            │ refactor systems    │            │ architecture        │
            └─────────┬───────────┘            └─────────┬───────────┘
                      │                                  │
                      └──────────────┬───────────────────┘
                                     ▼
                              Code Repository
                                  (GitHub)
                                     │
                                     ▼
                     ┌─────────────────────────────────┐
                     │          CI PIPELINE            │
                     │                                 │
                     │ Spec Verification               │
                     │ Impact Analysis (Spec Graph)    │
                     │ Scenario Exploration            │
                     │ Regression & Contract Tests     │
                     └───────────────┬─────────────────┘
                                     │
                                     ▼
                               CI/CD Merge Gate
                                     │
                                     ▼
                               Deployment
                                     │
                                     ▼
                            Production System
                                     │
                                     ▼
                       ┌─────────────────────────┐
                       │     Runtime Telemetry   │
                       │                         │
                       │ Events                  │
                       │ Errors                  │
                       │ User flows              │
                       └─────────────┬───────────┘
                                     │
                                     ▼
                         Spec Graph Updates
```

---

## Layer-by-Layer Explanation

### 1. Intent Layer (Source of Truth)

Engineers define structured behavioral specifications describing inputs, outputs, invariants, domain events, preconditions, and postconditions. This layer represents **what the product should do** — the [seed](../../../principles/seed.md) from which implementation grows.

**Example spec:**

```yaml
feature: create_invoice

inputs:
  - authenticated_user
  - invoice_items

invariants:
  - total == sum(invoice_items)

effects:
  - invoice_created
  - invoice_event_emitted
```

Spec verification tools treat these specs as authoritative artifacts for validation.

---

### 2. [Spec Graph](../../../GLOSSARY.md#spec-graph) (Behavior Model)

Specs compile into a graph of system behavior. This graph models domain workflows, event propagation, and system dependencies — becoming the **behavioral map of the product**.

**Example:**
```
create_invoice
   → capture_payment
   → ledger_entry
   → notification_sent
```

The spec graph serves a similar purpose to the structured flow documentation in [Codebase Cartography](../../../techniques/codebase-cartography.md), but in machine-readable form suitable for automated traversal and impact analysis.

---

### 3. [Behavioral Index](../../../GLOSSARY.md#behavioral-index)

The index connects the spec graph to implementation, enabling behavior-first navigation.

**Example mapping:**
```
create_invoice
  → InvoiceService.create()
  → POST /api/invoices
  → invoice_repository.save()
```

This extends [code indexing](../../../techniques/codebase-indexing.md) beyond symbol search into behavioral navigation — agents and CI systems can ask "what code implements this behavior?" rather than just "where is this function defined?"

---

### 4. Planning Engine

Specs convert into implementation plans — task DAGs that guide AI coding agents and developer workstreams.

**Example plan DAG:**
```
create_invoice
 ├ validate request
 ├ compute totals
 ├ persist invoice
 ├ emit event
 └ send notification
```

---

### 5. Implementation Layer

Features are implemented by developers, AI coding agents, or automated refactoring tools. Standard output artifacts: services, APIs, schemas, tests.

---

### 6. Verification Layer

Every change is verified against the behavioral model using complementary strategies:

| Strategy | Focus | Example tools |
|---|---|---|
| **Spec verification** | Implementation satisfies declared intent — invariant validation, API contract enforcement, policy compliance | Aviator Verify |
| **Scenario exploration** | Runtime robustness — state inconsistencies, UI regressions, integration issues | Ranger |
| **Impact analysis** | Graph traversal determines what needs testing based on behavioral dependencies | Custom (see [Pilot Guide](pilot-guide.md)) |

This layered verification approach applies [risk-tiered automation](../../../techniques/risk-tiered-automation.md): deterministic spec checks gate every PR, while deeper scenario exploration runs for changes affecting critical behavioral paths.

---

### 7. CI/CD Guardrails

Pull requests merge only when:

```
✓ spec constraints satisfied
✓ impacted behaviors verified
✓ scenario exploration passes
```

This turns CI/CD into **behavioral governance** — a concrete implementation of the [validation](../../../principles/validation.md) principle where the verifier operates independently of the implementer.

---

### 8. Runtime Feedback Loop

Production telemetry feeds back into the spec graph, closing the [feedback loop](../../../principles/feedback-loop.md):

| Trigger | Outcome |
|---|---|
| New user path detected | New scenarios added |
| Unexpected workflow discovered | New constraints identified |
| Rare edge case observed | New spec nodes created |

The system continuously improves its behavioral model — an application of [compound knowledge](../../../principles/compound-knowledge.md) where each production observation strengthens the specification base.

---

## The Architectural Shift

| Paradigm | Source of Truth |
|---|---|
| Traditional software | Code repository |
| Spec-driven software | Behavior model |

```
specs → behavior graph → code generation → behavior verification
```

Code becomes an **implementation artifact** inside a behavioral system. This parallels the broader pattern where the [environment](../../../principles/agent-native-environment.md) — not the agent — is the primary lever of effectiveness.

---

## See Also

- [Spec-Driven Architecture](spec-driven-architecture.md) — core concepts and the behavioral control plane
- [GitHub Workflow](github-workflow.md) — how this architecture operationalizes in practice
- [Adoption Roadmap](adoption-roadmap.md) — staged path to adopting this architecture
- [Pilot Guide](pilot-guide.md) — concrete repo structure and 90-day implementation plan
- [Codebase Cartography](../../../techniques/codebase-cartography.md) — structured documentation that the spec graph extends
- [Risk-Tiered Automation](../../../techniques/risk-tiered-automation.md) — graduated verification levels
