# Software Factory Knowledge Base — Table of Contents

This is the master index for the knowledge base. Every file in the project is reachable from here through section indexes.

**Start here:** [README.md](README.md) — The Software Factory Thesis

```
software-factory/
├── README.md                          — The software factory thesis
├── CLAUDE.md                          — Project instructions
├── INDEX.md                           — Master table of contents (this file)
├── SOURCES.md                         — Annotated bibliography
│
├── meta/                              — Context, paradigm, and commentary
│   ├── README.md
│   ├── meta.md                        — The moment, paradigm, and formula
│   ├── paradigm.md                    — Three lineages converging on the factory concept
│   ├── questions.md                   — Honest questions and design constraints
│   ├── agent-native/                  — Agent-native engineering in practice
│   │   ├── README.md
│   │   ├── cloud-agents.md            — Cloud agents and the background revolution
│   │   ├── case-studies.md            — Building in-house: Ramp and Stripe
│   │   ├── maturity-model.md          — Agent Readiness Model — maturity levels and pillars
│   │   └── practices/               — Agent-native practices catalog
│   │       └── README.md
│   ├── conversation.md               — Community commentary on the source material
│   ├── takeaways.md                  — Practical synthesis for engineering leaders
│   └── game-plan.md                  — Phased approach to agent-native adoption
│
├── principles/                        — Assumptions behind the software factory concept
│   ├── README.md
│   ├── seed.md                        — Specification as starting point
│   ├── validation.md                  — End-to-end proof of correctness
│   ├── feedback-loop.md               — Compounding correctness
│   ├── tokens-as-fuel.md              — Economics of agent labor
│   ├── deliberate-naivete.md          — Questioning inherited constraints
│   ├── compound-knowledge.md          — Organizational learning at machine speed
│   └── agent-native-environment.md    — Designing for non-human workers
│
├── techniques/                        — Repeatable patterns you can adopt
│   ├── README.md
│   ├── digital-twin-universe.md       — Cloning external dependencies for validation
│   ├── gene-transfusion.md            — Cross-codebase pattern transfer
│   ├── filesystem-as-memory.md        — Disk as agent cognition substrate
│   ├── shift-work.md                  — Interactive vs. non-interactive modes
│   ├── semport.md                     — Semantically-aware code migration
│   ├── progressive-disclosure.md      — Layered context management for agents
│   ├── pyramid-summaries.md           — Reversible multi-level context compression
│   ├── scenarios-not-tests.md         — Holdout-set validation replacing unit tests
│   ├── specification-discipline.md    — The self-check heuristic for specs
│   ├── risk-tiered-automation.md      — Graduated autonomy levels
│   ├── codebase-indexing.md           — Organization-wide code search and navigation
│   └── codebase-cartography.md       — Structured documentation as semantic index
│
├── components/                        — Infrastructure a software factory needs
│   ├── README.md
│   ├── attractor/                     — Pipeline orchestrator (56-file deep-dive)
│   │   ├── README.md
│   │   └── 00-overview/ ... 07-future-extensions/
│   ├── context-store/                 — Persistent structured memory for agents
│   │   ├── README.md
│   │   └── architecture.md
│   └── agent-identity/                — Auth/authz for mixed human-agent workforce
│       ├── README.md
│       └── architecture.md
│
└── implementations/                   — Known implementations
    ├── README.md
    └── kilroy.md                      — Local-first software factory CLI
```

---

## [Meta](meta/README.md)

Context, paradigm, and commentary surrounding the software factory concept.

| File | Description |
|------|-------------|
| [meta.md](meta/meta.md) | The moment, paradigm, and formula |
| [paradigm.md](meta/paradigm.md) | Three lineages converging on the factory concept |
| [questions.md](meta/questions.md) | Honest questions and design constraints |
| [agent-native/](meta/agent-native/README.md) | Agent-native engineering in practice — cloud agents, case studies, and maturity model |
| [conversation.md](meta/conversation.md) | Community commentary on the source material |
| [takeaways.md](meta/takeaways.md) | Practical synthesis for engineering leaders — evidence, organizational shifts, and what remains unproven |
| [game-plan.md](meta/game-plan.md) | Phased approach to agent-native adoption: assess, prepare, delegate, shift |

## [Principles](principles/README.md)

The assumptions behind the software factory concept. Seven principles forming a connected system.

| File | Description |
|------|-------------|
| [seed.md](principles/seed.md) | Specification as starting point — quality of seed determines output ceiling |
| [validation.md](principles/validation.md) | End-to-end proof of correctness, close to production |
| [feedback-loop.md](principles/feedback-loop.md) | Output fed back as input until convergence — compounding correctness |
| [tokens-as-fuel.md](principles/tokens-as-fuel.md) | Token economics as planning variable, not sunk cost |
| [deliberate-naivete.md](principles/deliberate-naivete.md) | Questioning inherited constraints — what was absurd is now routine |
| [compound-knowledge.md](principles/compound-knowledge.md) | Organizational learning at machine speed |
| [agent-native-environment.md](principles/agent-native-environment.md) | Designing environments for non-human workers |

## [Techniques](techniques/README.md)

Repeatable patterns to evaluate and adopt, clustered by domain: environment, workflow, and knowledge management.

| File | Description |
|------|-------------|
| [digital-twin-universe.md](techniques/digital-twin-universe.md) | Behavioral clones of third-party services for validation |
| [gene-transfusion.md](techniques/gene-transfusion.md) | Cross-codebase pattern transfer via exemplars |
| [filesystem-as-memory.md](techniques/filesystem-as-memory.md) | Disk as agent cognition substrate |
| [shift-work.md](techniques/shift-work.md) | Separating interactive from non-interactive agent modes |
| [semport.md](techniques/semport.md) | Semantically-aware code migration between languages/frameworks |
| [progressive-disclosure.md](techniques/progressive-disclosure.md) | Layered context management for agents |
| [pyramid-summaries.md](techniques/pyramid-summaries.md) | Reversible multi-level context compression |
| [scenarios-not-tests.md](techniques/scenarios-not-tests.md) | Holdout-set validation replacing unit tests |
| [specification-discipline.md](techniques/specification-discipline.md) | The self-check heuristic for specifications |
| [risk-tiered-automation.md](techniques/risk-tiered-automation.md) | Graduated autonomy levels for agent actions |
| [codebase-indexing.md](techniques/codebase-indexing.md) | Organization-wide code search, navigation, and impact analysis |
| [codebase-cartography.md](techniques/codebase-cartography.md) | Structured documentation as semantic index |

## [Components](components/README.md)

Infrastructure a software factory would need. Three architectural roles the concept implies.

| Directory | Description |
|-----------|-------------|
| [attractor/](components/attractor/README.md) | Pipeline orchestrator — DOT-based graph engine for multi-agent workflows (56-file deep-dive) |
| [context-store/](components/context-store/README.md) | Persistent structured memory for agents — DAG structure, deduplication, query patterns |
| [agent-identity/](components/agent-identity/README.md) | Auth/authz for mixed human-agent workforce — federated identity, audit trails |

## [Implementations](implementations/README.md)

Known implementations applying these ideas.

| File | Description |
|------|-------------|
| [kilroy.md](implementations/kilroy.md) | Dan Shapiro's Kilroy — local-first software factory CLI |

---

**Reference:** [SOURCES.md](SOURCES.md) — Annotated bibliography | [CLAUDE.md](CLAUDE.md) — Project conventions
