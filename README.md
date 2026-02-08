# The Software Factory Thesis

*So you want to build a software factory?*

It seems agentic coding has crossed an **inflection point** — models now [compound **correctness**](META.md) instead of error.

Three [intellectual threads](PARADIGM.md) appear to converge on one [**formula**](META.md#the-formula):

> **Seed** → **Validation** → **Feedback Loop**

…fueled by **tokens**.

The [hard **questions**](QUESTIONS.md) remain open.

The [**conversation**](CONVERSATION.md) is live.

---

- **[Meta](META.md)** — The moment, the paradigm, and the formula behind the software factory concept.
- **[Principles](principles/INDEX.md)** — The assumptions behind the software factory concept. Seven principles covering specification, validation, feedback, economics, and environment design.
- **[Techniques](techniques/INDEX.md)** — Repeatable patterns you can adopt. Ten techniques for environment construction, workflow design, and knowledge management.
- **[Components](components/INDEX.md)** — Infrastructure a software factory needs. Deep-dive on the Attractor pipeline orchestrator, plus architectural stubs for context stores and agent identity systems.
- **[Implementations](implementations/INDEX.md)** — People are building this now. Known implementations applying these principles in real projects.

---

```
software-factory/
├── README.md                          — "So you want to build a software factory"
├── CLAUDE.md                          — Project instructions
├── INDEX.md                           — Master table of contents
├── SOURCES.md                         — Annotated bibliography
├── PARADIGM.md                        — Three lineages converging on the factory concept
├── QUESTIONS.md                       — Honest questions and design constraints
├── CONVERSATION.md                    — Community discussion and essential reading
├── META.md                            — The moment, paradigm, and formula
│
├── principles/                        — Assumptions behind the software factory concept
│   └── ...7 principle files
│
├── techniques/                        — Repeatable patterns to evaluate and adopt
│   └── ...9 technique files
│
├── components/                        — Infrastructure a software factory needs
│   ├── attractor/                     — Pipeline orchestrator (deep-dive)
│   ├── context-store/                 — Persistent structured memory for agents
│   └── agent-identity/                — Auth/authz for mixed human-agent workforce
│
└── implementations/                   — Known implementations
    └── kilroy.md                      — Local-first software factory CLI
```
