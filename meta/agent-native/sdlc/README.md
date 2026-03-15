# Agentic SDLC — Knowledge Base

> A structured collection of documents covering spec-driven agentic SDLC concepts, architecture patterns, and adoption strategies. Derived from an in-depth conversation exploring how behavioral specifications, spec graphs, and AI coding agents combine to form a modern engineering platform.

---

## Documents

### [01 — Spec-Driven Agentic SDLC](./01-spec-driven-agentic-sdlc.md)

The foundational overview. Introduces the core principle of behavior-centric development and describes the four-layer architecture: Executable Specifications, Spec Graphs, Behavioral Index, and Behavioral CI Guardrails. Includes an adoption strategy and strategic outcome summary.

---

### [02 — Visual Architecture](./02-visual-architecture.md)

A detailed ASCII architecture diagram of the full spec-driven SDLC stack, with layer-by-layer explanations covering the Intent Layer, Spec Graph, Behavioral Index, Planning Layer, Implementation Layer, and Verification Layer (Aviator Verify + Ranger). Includes a one-sentence architectural takeaway.

---

### [03 — Spec-Gated GitHub Workflow](./03-spec-gated-github-workflow.md)

A GitHub-centric operationalization of the spec-driven model. Walks through the full development lifecycle from spec PR through CI merge gate, covering spec compilation, behavioral indexing, planning, implementation, and the three-stage CI verification pipeline (spec verification, impact analysis, scenario exploration).

---

### [04 — Behavioral Control Plane](./04-behavioral-control-plane.md)

The conceptual model that "unlocks" the architecture for engineering leadership. Explains why the behavioral control plane — the combination of spec graph and behavioral index — becomes the operating system for AI-native engineering teams, separating implementation, behavior modeling, intent, and verification into distinct planes.

---

### [05 — AI-Native SaaS Engineering Platform](./05-ai-native-saas-platform.md)

The complete end-to-end platform architecture. Combines all prior concepts into a unified diagram showing how product teams, intent layer, spec graph, behavioral index, planning engine, AI coding agents, CI/CD pipeline, and runtime telemetry form a closed feedback loop around behavioral correctness.

---

### [06 — Maturity Roadmap](./06-maturity-roadmap.md)

A four-stage adoption roadmap from test-centric development through fully agentic development. Describes the characteristics, capabilities, and limitations of each stage, with a realistic Year 1–3+ adoption timeline for SaaS organizations.

---

### [07 — One-Page Explainer](./07-one-page-explainer.md)

A single-slide condensation of the entire architecture suitable for CTO briefings and leadership presentations. Includes the consolidated architecture diagram, SDLC paradigm comparison table, tool placement (Aviator Verify, Ranger), and a ready-to-use one-sentence internal explanation.

---

### [08 — Repo Structure and 90-Day Pilot](./08-repo-structure-and-pilot.md)

A concrete implementation guide for teams ready to act. Covers the recommended GitHub monorepo layout (`specs/`, `graph/`, `behavior-index/`, CI workflows, and `AGENTS.md`), the spec file format, and a phased 90-day pilot scoped to a single domain. Includes working TypeScript for spec graph compilation, impact analysis, and spec verification. Delivers Stage 2 of the maturity model with Stage 3 infrastructure already in place.

---

## Key Concepts at a Glance

| Concept | Description |
|---|---|
| **Executable Spec** | Machine-readable behavioral contract defining inputs, outputs, invariants, and effects |
| **Spec Graph** | A directed graph of system behaviors connected by dependencies, events, and state transitions |
| **Behavioral Index** | A mapping from spec nodes to implementing code, enabling behavior-first navigation |
| **Planning Engine** | Converts specs into implementation task DAGs for developers and AI agents |
| **Spec Verification** | CI verification that implementation satisfies declared spec (e.g. Aviator Verify) |
| **Scenario Exploration** | Dynamic runtime testing that discovers emergent and unexpected behaviors (e.g. Ranger) |
| **Behavioral Control Plane** | The governance layer above code — specs + graph + index — that governs agentic development |

---

## The Core Progression

```
Traditional SDLC:   code → tests → deploy
Spec-Driven SDLC:   spec → code → verify
Agentic SDLC:       intent → plan → implement → verify
```
