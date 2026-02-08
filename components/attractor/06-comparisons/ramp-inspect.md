# Attractor vs Ramp Inspect

A detailed comparison of StrongDM's Attractor and Ramp's Inspect background coding agent platform.

## Overview

| | Attractor | Ramp Inspect |
|--|-----------|--------------|
| **Organization** | StrongDM | Ramp (fintech) |
| **Status** | NLSpec (specification only) | Production system (internal use) |
| **Public since** | Feb 2026 | Jan 2026 |
| **Philosophy** | Spec-driven, implementation-agnostic | Purpose-built for Ramp's workflows |

## Comparison by Dimension

### 1. Orchestration

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Model** | DOT-based directed graph with typed nodes | Linear agent sessions with spawnable child sessions |
| **Structure** | Explicit graph: nodes (tasks), edges (transitions), conditions | Sessions with tools; agents decide workflow structure |
| **Visibility** | Pipeline structure visible and visualizable before execution | Workflow emerges from agent behavior at runtime |
| **Configurability** | Pipeline behavior defined declaratively in DOT files | Agent behavior shaped by system prompts and available tools |

Attractor's graph-based approach makes the workflow structure **explicit and inspectable**. You can see all possible paths before running. Ramp's session-based approach lets agents **dynamically decide** what to do next, which is more flexible but less predictable.

### 2. Execution Environment

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Runtime** | Execution-environment-agnostic (local, Docker, K8s, WASM) | Cloud-native on Modal with sandboxed VMs |
| **Isolation** | Via pluggable ExecutionEnvironment interface | Built-in VM sandboxing per session |
| **Infrastructure** | Bring your own | Managed cloud infrastructure |

Ramp made a concrete choice (Modal VMs) that gives them strong isolation and scalability out of the box. Attractor's abstraction is more flexible but defers the infrastructure decision to implementors.

### 3. Verification

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Approach** | Scenario-based validation with goal gates and retry routing | Agents get full prod-equivalent tool access to self-verify |
| **Tools** | Goal gates enforce critical success criteria | Access to Sentry, Datadog, databases, internal APIs |
| **Pattern** | Pipeline retries until validation passes | Agent reads production signals to confirm correctness |

Ramp's approach is striking: agents can query production monitoring (Sentry error rates, Datadog metrics) to verify their own changes work. Attractor relies on explicit validation nodes and goal gates within the pipeline structure.

### 4. State Management

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Mechanism** | Checkpoint files + key-value context store | Cloudflare Durable Objects with per-session SQLite |
| **Persistence** | Implementation-defined (filesystem, DB, cloud storage) | Built into infrastructure (durable, replicated) |
| **Queryability** | Via checkpoint files and context inspection | SQL queries on session state |

Ramp's Durable Objects choice gives them automatic persistence and queryability. Attractor's checkpoint system is simpler but requires an explicit storage backend decision.

### 5. Human Interaction

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Model** | Formal Interviewer interface with typed questions | Multiplayer real-time collaboration |
| **When** | At explicit `wait.human` nodes in the graph | Continuous — humans can jump in anytime |
| **UX** | CLI, callbacks, or custom implementations | Slack bot, web IDE, Chrome extension |

Ramp's multiplayer model is more interactive — multiple people can observe and guide an agent session simultaneously, like pair programming. Attractor's approach is more structured — human interaction only happens at predefined decision points.

### 6. Interfaces

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Client UIs** | Spec-only (bring your own frontend) | Slack bot, web IDE (hosted VS Code), Chrome extension |
| **API** | Optional HTTP server mode | Built-in REST API + WebSocket |
| **Integration** | Via Interviewer interface and events | Direct integration with GitHub, Jira, Slack, Linear |

Ramp ships complete end-to-end interfaces. You can interact with Inspect via Slack, a web IDE, or a Chrome extension. Attractor is a backend system that needs frontend integration work.

### 7. Philosophy

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Approach** | Spec-driven, implementation-agnostic (NLSpec) | Purpose-built for Ramp's internal workflows |
| **Target** | Any organization wanting to build agent pipelines | Ramp's engineering organization |
| **Openness** | Specs published on GitHub | Architecture described in blog posts |
| **Reusability** | Designed for general adoption | Designed for Ramp's specific needs |

### 8. Context Handling

| Aspect | Attractor | Ramp Inspect |
|--------|-----------|--------------|
| **Model** | Configurable fidelity modes (full/compact/summary) | Not publicly documented in detail |
| **Cross-step** | Explicit context fidelity per edge/node | Session-based with shared workspace |

## What Each Approach Does Better

### Attractor Strengths

- **Inspectability**: The DOT graph makes the entire workflow visible before execution
- **Portability**: Spec-only approach works with any language, infrastructure, and LLM provider
- **Configurability**: Model stylesheet, context fidelity, and join policies provide fine-grained control
- **Generality**: Designed for any multi-stage AI workflow, not just one company's needs
- **Formal validation**: Goal gates and retry routing create principled success criteria

### Ramp Inspect Strengths

- **Production-proven**: Running in production at Ramp, handling real engineering tasks
- **End-to-end UX**: Complete interfaces (Slack, web IDE, Chrome extension) out of the box
- **Self-verification**: Agents can check production monitoring to verify their changes work
- **Real-time collaboration**: Multiplayer model enables interactive human guidance
- **Infrastructure decisions made**: Modal VMs, Durable Objects, Cloudflare — concrete, working choices
- **Integration depth**: Direct connections to GitHub, Jira, Sentry, Datadog

## Lessons for Our Implementation

1. **Start with concrete infrastructure choices**: Attractor's flexibility is a strength but also a liability — Ramp's concrete choices (Modal, Durable Objects) let them move faster
2. **Consider Slack as the first UI**: Ramp's Slack integration is their primary user interface; it's low-friction and asynchronous
3. **Production monitoring as verification**: Giving agents access to Sentry/Datadog for self-verification is powerful and worth considering
4. **Don't undervalue UX**: Ramp invested heavily in end-user interfaces; a great backend with no frontend has limited adoption
5. **Multiplayer as aspiration**: Real-time collaboration is compelling but can be added later — start with structured decision points

## Sources

- [Ramp Builders Blog: Why We Built Our Background Agent](https://builders.ramp.com/post/why-we-built-our-background-agent)
- [InfoQ Coverage: Ramp Coding Agent Platform](https://www.infoq.com/news/2026/01/ramp-coding-agent-platform/)
- [StrongDM Attractor Spec](https://github.com/strongdm/attractor)
- [StrongDM Software Factory](https://factory.strongdm.ai/)

## See Also

- [comparison-framework.md](comparison-framework.md) — The framework used for this analysis
- [../04-implementation-guide/decision-points.md](../04-implementation-guide/decision-points.md) — Our decision points, informed by this comparison
- [../07-future-extensions/collaboration-and-interfaces.md](../07-future-extensions/collaboration-and-interfaces.md) — Ramp-inspired interface ideas
