# Visual Architecture: Spec-Driven Agentic SDLC

> The four-layer stack (Intent → Planning → Implementation → Verification) and the spec graph + behavioral index control plane.

---

## Architecture Diagram

```
                ┌─────────────────────────────────┐
                │        PRODUCT DOMAINS          │
                │ Payments • Invoicing • Auth     │
                │ Orders • Documents • Billing    │
                └─────────────────┬───────────────┘
                                  │
                                  ▼
                     ┌────────────────────────┐
                     │        INTENT LAYER    │
                     │  Executable Specs      │
                     │  Behavioral Contracts  │
                     │  Policies / Invariants │
                     └─────────────┬──────────┘
                                   │
                                   ▼
                         ┌─────────────────────┐
                         │      SPEC GRAPH     │
                         │  System Behavior    │
                         │  Model (Nodes +     │
                         │  Behavioral Edges)  │
                         └───────────┬─────────┘
                                     │
            ┌────────────────────────┼────────────────────────┐
            ▼                        ▼                        ▼
   Behavioral Index           Planning Engine          Impact Analysis
   (Spec → Code Map)          (Spec → Task Graph)      (Graph Traversal)
            │                        │                        │
            ▼                        ▼                        ▼
  ┌────────────────┐     ┌───────────────────┐   ┌──────────────────┐
  │ Implementation │     │   Implementation  │   │  Behavioral CI   │
  │     Layer      │     │       Agents      │   │   Guardrails     │
  │ Developers +   │     │  AI Coding Agents │   │ Graph-aware tests│
  │ AI Assistants  │     │                   │   │ & verification   │
  └─────────┬──────┘     └─────────┬─────────┘   └─────────┬────────┘
            │                      │                        │
            └──────────────┬───────┴──────────────┬─────────┘
                           ▼                      ▼
                ┌────────────────────┐   ┌────────────────────┐
                │ Spec Verification  │   │ Scenario Exploration│
                │ (intent checks)    │   │ (runtime discovery) │
                │                    │   │                    │
                │ Aviator Verify     │   │ Ranger             │
                └──────────┬─────────┘   └──────────┬─────────┘
                           │                        │
                           └──────────────┬──────────┘
                                          ▼
                               ┌───────────────────┐
                               │   CI / CD Merge   │
                               │ Behavioral Gates  │
                               │ Spec → Tests → QA │
                               └──────────┬────────┘
                                          ▼
                               ┌───────────────────┐
                               │ Production System │
                               │ Runtime Telemetry │
                               └───────────────────┘
```

---

## Layer-by-Layer Explanation

### 1. Intent Layer (Specs)

Engineers define structured behavioral specifications describing:
- Inputs / outputs
- Invariants
- Domain events
- Preconditions and postconditions

This becomes the **source of truth** for system behavior.

---

### 2. Spec Graph (System Model)

Specs connect into a behavior graph.

**Example:**
```
create_invoice → capture_payment → ledger_entry
```

This graph represents:
- Workflows
- Domain dependencies
- Event propagation

The spec graph becomes the **behavioral model of the product**.

---

### 3. Behavioral Index

The spec graph becomes a navigation layer over the codebase.

Instead of searching code via embeddings:
```
query → files
```

Agents navigate via behavior:
```
query → spec node → related behaviors → implementation
```

This greatly improves reliability for AI-assisted changes.

---

### 4. Planning Layer

A planning system converts specs into implementation graphs.

**Example:**
```
create_invoice
 ├ validate request
 ├ compute totals
 ├ persist invoice
 └ emit event
```

These plans guide developers, AI coding agents, and automated refactoring.

---

### 5. Implementation Layer

Both humans and AI agents generate code guided by specs, plans, and the behavioral index.

This layer produces standard artifacts:
- Source code
- APIs
- Database schema
- Tests

---

### 6. Verification Layer

Two complementary approaches verify that implementation matches intent:

| Approach | System | Focus |
|---|---|---|
| **Spec Verification** | Aviator Verify | Implementation correctness against spec |
| **Scenario Exploration** | Ranger | Runtime robustness and emergent behavior |

Together they verify both **intended behavior** and **emergent behavior**.

---

## The Big Architectural Shift

**Traditional SDLC:**
```
code → tests → deploy
```

**Agentic SDLC:**
```
intent → plan → implement → behavioral verification
```

Code becomes an **implementation artifact** rather than the primary system model.

---

## One-Sentence Takeaway

> A modern SaaS engineering platform should treat spec graphs as the behavioral control plane, allowing AI agents to implement features while CI pipelines enforce correctness through spec verification and exploratory testing.
