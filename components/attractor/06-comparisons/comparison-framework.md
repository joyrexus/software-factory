# Comparison Framework

Dimensions for evaluating and comparing agent architectures in the background coding agent space. Each comparison file in this directory should address these axes.

## Evaluation Dimensions

### 1. Orchestration Model

How the system structures multi-step work:

| Approach | Description | Example |
|----------|-------------|---------|
| **Directed graph** | Explicit nodes and edges with typed transitions | Attractor |
| **Linear sequence** | Steps executed in order | Simple agent pipelines |
| **Tree/hierarchy** | Parent tasks spawn child tasks | Agent with subagents |
| **Event-driven** | Tasks triggered by events/conditions | Reactive systems |

### 2. Execution Environment

Where agent code actually runs:

| Approach | Characteristics |
|----------|----------------|
| **Local** | Developer's machine, full access |
| **Cloud-sandboxed** | Remote VMs, isolated per session |
| **Container-based** | Docker/K8s, reproducible environments |
| **WASM** | Lightweight sandbox, limited I/O |
| **Hybrid** | Multiple environments per workflow |

### 3. Verification Approach

How the system proves agent output is correct:

| Approach | Description |
|----------|-------------|
| **Test-based** | Run existing test suites |
| **Scenario-based** | Behavioral validation against acceptance criteria |
| **Monitoring-based** | Deploy and observe production behavior |
| **Human review** | Human validates output before accepting |
| **Self-verification** | Agent checks its own work with tools |

### 4. Human-in-the-Loop Model

How and when humans interact:

| Approach | Description |
|----------|-------------|
| **Structured decision points** | Predefined moments for human input |
| **Real-time collaboration** | Continuous interaction during execution |
| **Async approval** | Queue-based, respond when available |
| **Fully autonomous** | No human interaction during execution |

### 5. State Management

How execution state is tracked and persisted:

| Approach | Description |
|----------|-------------|
| **Key-value context** | Simple map carried through execution |
| **Checkpoint files** | Periodic state snapshots to disk |
| **Database-backed** | Durable objects or relational storage |
| **In-memory only** | State lost on crash |

### 6. Context Handling

How the system manages LLM context across steps:

| Approach | Description |
|----------|-------------|
| **Full history** | Complete conversation carries forward |
| **Summarized** | Prior context condensed to summaries |
| **Fresh sessions** | Each step starts with clean context |
| **Fidelity modes** | Configurable per-step context management |

### 7. Multi-Model Support

How the system works with different LLM providers:

| Approach | Description |
|----------|-------------|
| **Single provider** | Hardcoded to one LLM |
| **Provider-agnostic** | Unified interface across providers |
| **Per-task routing** | Different models for different task types |

### 8. Extensibility

How the system can be extended:

| Approach | Description |
|----------|-------------|
| **Plugin architecture** | Register custom components |
| **Configuration-driven** | Customize via config files |
| **API-first** | External systems integrate via APIs |
| **Fork-and-modify** | Custom behavior requires code changes |

### 9. Deployment Model

How the system is deployed and operated:

| Approach | Description |
|----------|-------------|
| **Specification only** | Bring your own implementation |
| **SaaS** | Hosted service |
| **Self-hosted** | Deploy on your infrastructure |
| **CLI tool** | Developer workstation tool |

## Using This Framework

When writing a comparison:

1. State each system's position on each dimension
2. Highlight the most significant differences
3. Note what each approach does better
4. Identify lessons that could inform our implementation
5. Cite sources

## See Also

- [ramp-inspect.md](ramp-inspect.md) — First comparison using this framework
