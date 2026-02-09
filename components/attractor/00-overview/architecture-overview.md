# Architecture Overview

The three NLSpec components form a layered stack where each layer builds on the one below it.

## The Stack

```
┌─────────────────────────────────────────────┐
│         Attractor Pipeline Orchestrator      │
│  DOT graph → multi-phase workflow execution  │
│         Creates and manages sessions ↓       │
├─────────────────────────────────────────────┤
│           Coding Agent Loop                  │
│  LLM ↔ tool cycle with truncation,          │
│  loop detection, and steering                │
│         Uses LLM client for model calls ↓    │
├─────────────────────────────────────────────┤
│           Unified LLM Client                 │
│  Provider-agnostic access to                 │
│  OpenAI, Anthropic, Gemini                   │
└─────────────────────────────────────────────┘
```

## Dependency Chain

**Unified LLM Client** (no external dependencies within the stack)
- Provides `Client.complete()` and `Client.stream()` for any supported provider
- Handles authentication, retries, streaming, tool calling protocol
- The foundation — everything else talks to models through this

**Coding Agent Loop** (depends on: LLM Client)
- Uses the LLM Client as the `CodergenBackend` — the interface for making LLM calls
- Adds the agentic loop: send prompt → get tool calls → execute tools → send results → repeat
- Manages session state, output truncation, loop detection, steering context

**Attractor Pipeline** (depends on: Coding Agent Loop, LLM Client)
- Uses the Coding Agent Loop to execute `codergen` nodes (the default node type)
- Manages the graph traversal, edge routing, state context, checkpointing
- Orchestrates multiple agent sessions across a directed graph of phases

## Key Integration Points

| Integration | Interface | Description |
|-------------|-----------|-------------|
| Pipeline → Agent Loop | `CodergenBackend` | Pipeline creates agent sessions for each codergen node |
| Agent Loop → LLM Client | `Client` / provider adapters | Agent loop makes LLM calls through the unified client |
| Pipeline → LLM Client | Model stylesheet | Pipeline configures which models to use via CSS-like rules |
| Pipeline → Agent Loop | Context fidelity | Pipeline controls how much session history carries between nodes |

## Data Flow Example

A single node execution in a pipeline:

1. **Pipeline** reads the DOT graph, selects the next node, prepares the context
2. **Pipeline** creates a `CodergenBackend` session with the node's goal and context
3. **Agent Loop** sends the system prompt + goal to the LLM via the **LLM Client**
4. **LLM Client** routes to the correct provider adapter (e.g., Anthropic), makes the API call
5. **Model** responds with tool calls (e.g., read file, edit file)
6. **Agent Loop** executes the tools in the execution environment, sends results back
7. Steps 4-6 repeat until the model signals completion or a stop condition is hit
8. **Agent Loop** returns the session outcome to the **Pipeline**
9. **Pipeline** updates context, saves a checkpoint, selects the next edge to traverse

## Build Implications

This layering directly determines the build sequence — you must build bottom-up:

1. **LLM Client first** — no dependencies, independently testable against live APIs
2. **Agent Loop second** — needs the LLM client for its backend
3. **Pipeline last** — needs the agent loop for node execution

See [../04-implementation-guide/build-sequence.md](../04-implementation-guide/build-sequence.md) for detailed build guidance.

## See Also

- [../01-pipeline-orchestrator/README.md](../01-pipeline-orchestrator/README.md) — Deep dive into the orchestrator
- [../02-coding-agent-loop/README.md](../02-coding-agent-loop/README.md) — Deep dive into the agent loop
- [../03-unified-llm-client/README.md](../03-unified-llm-client/README.md) — Deep dive into the LLM client
- [../04-implementation-guide/integration-patterns.md](../04-implementation-guide/integration-patterns.md) — How the components connect in practice
