# Adoption Roadmap

*Staged progression from test-centric development to spec-driven agentic SDLC, mapped to the Agent Readiness Model.*

## Overview

Adopting a spec-driven architecture is not a single transition — it is a staged progression where each stage builds capabilities that the next stage requires. This roadmap describes four stages, maps each to the [Agent Readiness Model](../maturity-model.md) levels and pillars, identifies gating criteria for advancement, and calls out the failure modes practitioners encounter at each stage.

---

## The Four Stages

```
Stage 1 — Test-Centric SDLC
     │
     ▼
Stage 2 — Executable Specs
     │
     ▼
Stage 3 — Behavioral Graph Platform
     │
     ▼
Stage 4 — Spec-Driven Agentic Development
```

Each stage builds on the previous one.

---

## Stage-to-ARM Mapping

The [Agent Readiness Model](../maturity-model.md) defines five maturity levels (L1–L5) and ten technical pillars. The SDLC stages represent a *workflow progression* through those levels — they describe what the development process looks like as the codebase matures through readiness levels.

| SDLC Stage | ARM Levels | Exercised Pillars | Rationale |
|---|---|---|---|
| Stage 1 (Test-Centric) | L1–L2 (Functional–Documented) | Testing, Build System, Style & Validation | Code runs, basic automation exists, some documentation |
| Stage 2 (Executable Specs) | L2–L3 (Documented–Standardized) | Task Discovery, Documentation, Testing, Security | Processes defined and enforced through spec-based automation |
| Stage 3 (Behavioral Graph) | L3–L4 (Standardized–Optimized) | Codebase Indexing, Debugging & Observability, Product & Experimentation | Fast feedback loops, graph-aware analysis, data-driven improvement |
| Stage 4 (Agentic Development) | L4–L5 (Optimized–Autonomous) | All pillars | Self-improving systems with sophisticated orchestration |

The ARM's gating rule — advancement requires 80% of criteria at current level and all previous levels — applies here: don't advance to Stage 3 without strong Stage 2 fundamentals.

---

## Stage 1 — Test-Centric Development

**Typical development loop:**
```
code → tests → CI → deploy
```

**Characteristics:**
- Unit tests, integration tests, BDD or scenario tests
- CI pipelines validating PRs
- System model lives in code and human understanding

This is where most engineering organizations begin. Scenario exploration tools can already add value here by discovering runtime edge cases, and [linters as architectural guardrails](../practices/README.md#linters-as-architectural-guardrails) provide machine-enforced quality.

**Limitations that motivate Stage 2:**
- Tests drift from requirements over time
- Architecture knowledge is implicit — held in people's heads
- Impact analysis is manual and error-prone
- AI coding agents struggle to navigate large systems without structured context

**Gating criteria to advance:**
- CI runs reliably on every PR with clear pass/fail signals
- ≥80% of critical paths have automated test coverage
- Build system is reproducible and documented
- Basic [codebase cartography](../../../techniques/codebase-cartography.md) exists (component docs, flow docs)

**Failure modes:**
- *Testing theater* — high coverage numbers masking tests that verify implementation details rather than behavior
- *CI flakiness* — intermittent failures that train engineers to ignore CI signals

---

## Stage 2 — Executable Specs

This stage introduces machine-readable specifications as first-class artifacts — a concrete application of [specification discipline](../../../techniques/specification-discipline.md).

**Development loop:**
```
spec → implementation → spec verification
```

Specs describe inputs, outputs, invariants, domain rules, and expected events. Spec verification tools (e.g., Aviator Verify) validate implementation against these specifications.

**What changes:** Instead of relying only on tests, teams begin treating behavioral intent as structured data. Specs serve triple duty: instruction to agents, acceptance criteria for CI, and behavioral documentation for engineers.

**Benefits:**
- Clearer product contracts
- Easier review of feature behavior
- Automated spec validation
- Foundation for agent-ready [seeds](../../../principles/seed.md)

**Gating criteria to advance:**
- ≥80% of new features have machine-readable specs
- Spec verification runs in CI for ≥1 bounded domain
- Spec format is standardized and documented
- Engineers and PMs are authoring specs (not just engineers)

**Failure modes:**
- *Spec drift* — specifications decouple from implementation when authoring is not part of the code review process. Mitigate by requiring spec PR before implementation PR.
- *Over-specification* — specs that prescribe implementation details rather than behavior, creating brittle contracts that break on refactoring
- *Spec orphans* — specs written for new features while existing features remain undocumented, creating an inconsistent hybrid

**The [90-day pilot](pilot-guide.md) is designed as the entry path to Stage 2** — it delivers executable specs, a compiled spec graph, and spec verification for a single bounded domain.

---

## Stage 3 — Behavioral Graph Platform

This is where the architecture becomes significantly more powerful. Executable specs compile into a **[spec graph](../../../GLOSSARY.md#spec-graph)** representing system behavior across domains.

**Example:**
```
create_invoice
   → capture_payment
   → ledger_update
   → notification_sent
```

### New Capabilities

**[Behavioral indexing](../../../GLOSSARY.md#behavioral-index)** maps behaviors to code — extending [codebase indexing](../../../techniques/codebase-indexing.md) into behavioral navigation:
```
create_invoice
   → InvoiceService.create()
   → POST /api/invoices
```

**Graph-aware CI** uses the spec graph to scope verification:
```
change → affected behaviors → required verification
```

Scenario exploration tools complement deterministic spec validation, together addressing both intended and [emergent behavior](../../../techniques/scenarios-not-tests.md).

**Organizational impact:** Engineering teams begin thinking in terms of behavior flows and domain models, not just services and endpoints. The spec graph becomes a shared language between PMs, engineers, and AI agents.

**Gating criteria to advance:**
- Spec graph covers ≥2 bounded domains with cross-domain edges
- Behavioral index links ≥80% of spec nodes to implementation
- Impact analysis runs automatically on code PRs
- Graph compilation is part of CI (not a manual process)

**Failure modes:**
- *Graph staleness* — the behavioral index falls out of sync when spec compilation is not part of CI. The graph must be generated, not manually maintained.
- *Domain silos* — teams build isolated spec graphs that don't connect at domain boundaries, missing the cross-domain impact analysis that provides the most value
- *Index drift* — behavioral index mappings become stale when code is refactored without updating the spec-to-code mapping. [Entropy management](../practices/README.md#entropy-management) practices help.

---

## Stage 4 — Spec-Driven Agentic Development

At this stage, the spec graph becomes the **control plane for development** — agents navigate, plan, implement, and verify against the behavioral model.

**Development loop:**
```
intent → plan → implement → verify
```

### Key Components

| Component | Role |
|---|---|
| **Intent Layer** | Executable specs define system behavior |
| **Planning Layer** | AI planners convert specs into task graphs |
| **Implementation Layer** | AI coding agents generate and modify code |
| **Verification Layer** | CI/CD pipelines enforce behavioral correctness |

### Human Role Shifts

Engineers focus on architecture, domain modeling, spec authoring, and reviewing agent-generated changes. AI agents handle much of the mechanical implementation. This parallels the broader shift described in [Cloud Agents](../cloud-agents.md) — engineers move from writing code to managing agents.

**Gating criteria:**
- Planning engine generates implementation plans from specs
- AI agents successfully implement features using behavioral index navigation
- Runtime telemetry feeds back into spec graph updates
- All ARM pillars at L4+ (80% criteria)

**Failure modes:**
- *Agent over-trust* — accepting agent output without behavioral verification, losing the independent validation that makes the model safe
- *Spec maintenance burden* — as the system grows, keeping specs current becomes a significant cost. Dedicated [entropy management](../practices/README.md#entropy-management) practices and background agents for spec gardening are essential.
- *Feedback loop decay* — runtime telemetry stops flowing into spec updates, and the behavioral model gradually diverges from production reality

---

## Legacy Code Strategy

Most organizations adopting this model have large existing codebases that predate any specification discipline. The pragmatic approach is **incremental, not retroactive**:

1. **New features first.** All new features require specs from day one. This is low-friction and immediately valuable.
2. **High-value existing features next.** Prioritize features that are frequently modified, have high business impact, or are targets for AI agent work. Writing specs for these features forces clarity about what the system actually does — itself a valuable exercise.
3. **Discovery through modification.** When engineers or agents modify existing code, require a spec for the affected behavior as part of the change. Over time, the most actively maintained code accumulates specs organically.
4. **Don't boil the ocean.** Comprehensive retroactive spec-writing for an entire codebase is rarely justified. The goal is coverage where it matters, not 100% coverage everywhere.

This mirrors the approach recommended for [codebase cartography](../../../techniques/codebase-cartography.md) — document what you touch, don't try to document everything at once.

---

## Organizational Considerations

**Who authors specs?** At Stage 2, primarily engineers translating requirements into structured format. At Stage 3+, PMs and architects increasingly author specs directly, with engineers reviewing for technical accuracy. The spec format should be accessible to non-engineers — YAML with clear semantics, not code.

**How does this change the PM↔engineer interface?** Specs become the handoff artifact. Instead of Jira tickets with prose descriptions, PMs produce structured behavioral contracts. This is a significant process change that requires PM buy-in and training.

**What skills are needed?** Domain modeling becomes a core engineering skill. Engineers must think in terms of behaviors, invariants, and effects — not just functions and endpoints. BDD experience is helpful but not required.

**What's the ongoing cost?** Spec authoring adds overhead to feature development. Practitioners report this cost is offset by reduced ambiguity, fewer rework cycles, and faster AI agent onboarding — but the initial learning curve is real.

---

## Realistic Adoption Timeline

| Timeframe | Focus | SDLC Stage |
|---|---|---|
| **Months 1–3** | Run the [90-day pilot](pilot-guide.md): specs for one domain, compiled graph, spec verification in CI | Stage 2 entry |
| **Months 4–9** | Expand to 2–3 domains, build behavioral index, implement impact analysis | Stage 2 → Stage 3 |
| **Months 9–18** | Graph-aware CI across major domains, AI agent navigation via behavioral index | Stage 3 |
| **Year 2+** | Planning engine, agentic implementation, runtime feedback loop | Stage 3 → Stage 4 |

These timelines assume a mid-size SaaS engineering team (20–80 engineers). Smaller teams may move faster; larger organizations may need longer for organizational alignment.

---

## See Also

- [Agent Readiness Model](../maturity-model.md) — the maturity framework these stages map to
- [Pilot Guide](pilot-guide.md) — concrete 90-day plan for entering Stage 2
- [Spec-Driven Architecture](spec-driven-architecture.md) — the architecture this roadmap adopts
- [Specification Discipline](../../../techniques/specification-discipline.md) — the self-check heuristic for writing agent-ready specs
- [Entropy Management](../practices/README.md#entropy-management) — continuous cleanup practices essential at Stage 3+
- [Cloud Agents](../cloud-agents.md) — the delegation model that Stage 4 enables
- [Takeaways](../../takeaways.md) — broader adoption guidance for engineering leaders
