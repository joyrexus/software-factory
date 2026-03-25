# Spec-Driven Agentic SDLC

*A workflow architecture composing agent-native practices into an end-to-end development lifecycle.*

---

The [practices catalog](../practices/README.md) maps *what* to adopt — linters, code indexing, background agents, entropy management. This section maps *how* these practices compose into a coherent development workflow built around behavioral specifications.

The central architectural idea: system behavior, expressed as structured executable specifications compiled into a spec graph, becomes the control plane for AI-assisted software development. Code becomes the implementation layer; the spec graph becomes the behavioral model of the product. This approach instantiates several principles from the broader framework — [specification discipline](../../../techniques/specification-discipline.md) for writing agent-ready specs, [codebase cartography](../../../techniques/codebase-cartography.md) for structured system documentation, and [validation](../../../principles/validation.md) for independent behavioral verification.

## The Core Progression

```
Traditional SDLC:   code → tests → deploy
Spec-Driven SDLC:   spec → code → verify
Agentic SDLC:       intent → plan → implement → verify
```

## Documents

| File | Description |
|------|-------------|
| [clarity-constraint.md](clarity-constraint.md) | Argumentative foundation — six claims about why specs are the central artifact and the synthesis they produce |
| [spec-driven-architecture.md](spec-driven-architecture.md) | Core concepts — the four-layer architecture and behavioral control plane |
| [architecture-reference.md](architecture-reference.md) | Visual reference — closed-loop platform diagram with layer-by-layer explanation |
| [github-workflow.md](github-workflow.md) | Operationalization — spec-gated GitHub workflow from spec PR through verification to deployment |
| [adoption-roadmap.md](adoption-roadmap.md) | Staged adoption — four stages mapped to the [Agent Readiness Model](../maturity-model.md), with gating criteria and failure modes |
| [one-page-explainer.md](one-page-explainer.md) | Executive summary — single-slide architecture view for CTO briefings |
| [pilot-guide.md](pilot-guide.md) | Implementation guide — concrete repo structure, spec format, and 90-day pilot plan with working TypeScript |

## Key Concepts

| Concept | Description |
|---|---|
| **[Executable Spec](../../../GLOSSARY.md#executable-spec)** | Machine-readable behavioral contract defining inputs, outputs, invariants, and effects |
| **[Spec Graph](../../../GLOSSARY.md#spec-graph)** | A directed graph of system behaviors connected by dependencies, events, and state transitions |
| **[Behavioral Index](../../../GLOSSARY.md#behavioral-index)** | A mapping from spec nodes to implementing code, enabling behavior-first navigation |
| **[Behavioral Control Plane](../../../GLOSSARY.md#behavioral-control-plane)** | The governance layer above code — specs + graph + index — that governs agentic development |

These terms are defined in the [Glossary](../../../GLOSSARY.md).

## Connection to the Broader Framework

The spec-driven SDLC directly instantiates several existing practices and techniques:

| SDLC Concept | Existing Practice/Technique | Connection |
|---|---|---|
| Executable specs as CI gate | [Linters as Architectural Guardrails](../practices/README.md#linters-as-architectural-guardrails) | Spec verification is a behavioral linter |
| Behavioral index | [Code Indexing and Search](../practices/README.md#code-indexing-and-search) | Spec-to-code mapping extends code search into behavioral navigation |
| Spec graph as documentation | [Codebase Cartography](../../../techniques/codebase-cartography.md) | The spec graph is a machine-readable cartographic artifact |
| AI agents navigating via index | [Background Agents](../practices/README.md#background-agents) | The behavioral index is the context layer background agents need |
| Spec drift detection | [Entropy Management](../practices/README.md#entropy-management) | Spec-code drift is a form of entropy requiring continuous cleanup |
| Spec format | [Specification Discipline](../../../techniques/specification-discipline.md) | Executable specs are a concrete implementation of the self-check heuristic |

---

**See also:**
- [Agent-Native Engineering](../README.md) — the parent directory: what agents need and how organizations are providing it
- [Agent Readiness Model](../maturity-model.md) — maturity framework the adoption roadmap maps to
- [Practices Catalog](../practices/README.md) — the individual practices this workflow composes
- [Takeaways](../../takeaways.md) — broader adoption guidance for engineering leaders
