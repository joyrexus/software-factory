# One-Page Explainer

*A condensed architecture view suitable for CTO briefings, engineering RFCs, and internal strategy decks.*

## Architecture Overview

```
                     PRODUCT INTENT
              (What the system should do)

        Executable Specifications
        Domain Rules & Invariants
        Behavioral Contracts
                     │
                     ▼
             ┌────────────────┐
             │   SPEC GRAPH   │
             │                │
             │ System behavior│
             │ model of the   │
             │ entire product │
             └───────┬────────┘
                     │
      ┌──────────────┼──────────────┐
      ▼              ▼              ▼
Behavioral      Planning       Impact
  Index          Engine        Analysis
(Spec ↔ Code)  (Spec → Tasks) (Graph traversal)
      │              │              │
      ▼              ▼              ▼
  AI Coding Agents  +  Human Engineers
              implement features
                     │
                     ▼
                 CODEBASE
                 (GitHub)
                     │
                     ▼
              CI / CD PIPELINE
    ┌───────────────────────────────────┐
    │ Spec Verification                 │
    │ (intent correctness)              │
    │                                   │
    │ Scenario Exploration              │
    │ (runtime discovery)               │
    │                                   │
    │ Behavioral Impact Testing         │
    │ (graph-aware validation)          │
    └───────────────┬───────────────────┘
                    ▼
               MERGE GATE
                    │
                    ▼
                DEPLOYMENT
                    │
                    ▼
            PRODUCTION SYSTEM
                    │
                    ▼
           Runtime Telemetry
                    │
                    ▼
          Spec Graph Evolution
```

---

## The Core Idea

| Paradigm | Development Model |
|---|---|
| **Traditional SDLC** | `code → tests → deploy` |
| **Spec-Driven SDLC** | `spec → code → verify` |
| **Agentic SDLC** | `intent → plan → implement → verify` |

Traditional software development revolves around **code**. Emerging practice suggests that AI-native development increasingly revolves around **behavior models** — structured specifications that define what the system should do, independent of how it is implemented.

---

## The Architectural Shift

Instead of relying on human code review and large regression suites, spec-driven engineering relies on:

- **Explicit behavioral models** — the spec graph serves as a machine-readable system map
- **Graph-aware verification** — impact analysis determines what needs testing
- **Spec-gated CI/CD** — behavioral correctness as the merge criterion

The spec graph becomes the behavioral control plane for the system — a concrete implementation of the principles described in [Spec-Driven Architecture](spec-driven-architecture.md).

---

## What This Means for a SaaS Organization

Engineering teams shift from **code-centric development** to **behavior-centric development**. The primary artifacts become executable specs, spec graphs, and behavioral indexes. These artifacts guide AI coding agents, CI/CD verification, and system evolution.

For how this maps to organizational maturity, see the [Adoption Roadmap](adoption-roadmap.md). For a concrete starting point, see the [90-Day Pilot Guide](pilot-guide.md).

---

## One-Sentence Explanation

> An engineering platform where executable behavioral specifications form a system-wide spec graph that guides AI implementation, impact analysis, and CI/CD verification — shifting the source of truth from code to behavior.

---

## See Also

- [Spec-Driven Architecture](spec-driven-architecture.md) — full conceptual treatment
- [Architecture Reference](architecture-reference.md) — detailed platform diagram with layer explanations
- [Adoption Roadmap](adoption-roadmap.md) — staged adoption path mapped to the Agent Readiness Model
- [Pilot Guide](pilot-guide.md) — concrete repo structure and 90-day implementation plan
