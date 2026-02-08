# Software Factory Context

Attractor is one product within StrongDM's broader Software Factory vision — a system for building software using AI agents as the primary production mechanism.

## The Software Factory Model

The Software Factory is built on a growth loop:

```
Seed → Validation → Feedback Loop → Tokens
```

1. **Seed**: A natural language specification (NLSpec) describing what to build
2. **Validation**: Automated mechanisms proving the output works (tests, scenarios, DTU)
3. **Feedback Loop**: Results feed back to refine the next iteration
4. **Tokens**: The raw material — LLM API calls that do the actual generation work

The key insight is that the bottleneck shifts from **writing code** to **specifying what to build** and **validating what was built**. The coding itself is delegated to agents.

## Interactive vs Non-Interactive Growth

The factory distinguishes two modes:

| Mode | Description | Example |
|------|-------------|---------|
| **Interactive** | Human-in-the-loop, real-time collaboration | Using Claude Code or Cursor to pair-program |
| **Non-interactive** | Fully specified, runs autonomously | Attractor pipeline executing a multi-phase coding task |

Attractor targets the **non-interactive** mode — what the factory calls **shift work**. You define the pipeline (the DOT graph), specify the goals, and hand it off. The agent runs through the phases, using human interaction only at explicit decision points (the `wait.human` handler).

## Core Principles

The Software Factory site articulates several principles relevant to understanding Attractor:

- **Specification over implementation**: Write specs, not code. The agent implements.
- **Filesystem as memory**: On-disk state (files, directories, artifacts) is the working memory for both humans and agents.
- **Genrefying**: Reorganize knowledge structures as understanding deepens, following library-science principles.
- **Validation is the hard part**: Generation is (increasingly) solved; proving the output is correct is the real challenge.
- **Tokens are cheap, verification is expensive**: Optimize for confidence in outputs, not for minimizing LLM calls.

## Where Attractor Fits

Within the Software Factory product lineup:

- **Attractor** — The pipeline orchestrator (the subject of this knowledge base)
- **CXDB** — Conversation-native observability database
- **Semport** — Semantic import/export system
- **DTU (Digital Twin Universe)** — Behavioral clones of third-party services for testing

Attractor is the **execution engine** — it's how the factory actually runs coding tasks. The other products provide supporting capabilities (observability, data movement, test infrastructure).

## See Also

- [what-is-attractor.md](what-is-attractor.md) — What the system does concretely
- [architecture-overview.md](architecture-overview.md) — The technical stack
- [../07-future-extensions/validation-and-testing.md](../07-future-extensions/validation-and-testing.md) — DTU and advanced validation
