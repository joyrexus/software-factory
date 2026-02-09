# The Software Factory Thesis

Several teams have reported that agentic coding workflows — where LLMs write, test, and iterate on code autonomously — can [compound correctness](META.md) rather than error.

Three [intellectual threads](PARADIGM.md) appear to converge on a shared pattern:

> **Seed** → **Validation** → **Feedback Loop**

…fueled by **tokens**.

The [hard questions](QUESTIONS.md) remain open.

---

- **[Meta](META.md)** — The moment, the paradigm, and the formula behind the software factory concept.
- **[Principles](principles/README.md)** — The assumptions behind the software factory concept. Seven principles covering specification, validation, feedback, economics, and environment design.
- **[Techniques](techniques/README.md)** — Repeatable patterns to evaluate and adopt. Ten techniques for environment construction, workflow design, and knowledge management.
- **[Components](components/README.md)** — Infrastructure a software factory would need. Deep-dive on the Attractor pipeline orchestrator, plus architectural stubs for context stores and agent identity systems.
- **[Implementations](implementations/README.md)** — Known implementations applying these ideas.
- **[Conversation](CONVERSATION.md)** — Community commentary on the source material.

---

```
software-factory/
├── README.md                          — The software factory thesis
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
