# Build Sequence

The three components should be built bottom-up, following the dependency chain.

## Recommended Order

### Phase 1: Unified LLM Client

**Build first. No internal dependencies.**

The LLM client is the foundation — both the Coding Agent Loop and the Attractor Pipeline depend on it, but it depends on neither.

**What to build:**
1. Message and content model (data structures)
2. Error hierarchy and retry logic
3. Provider adapters (start with one, add others)
4. Core Client class with routing and middleware
5. High-level API (generate, stream, generate_object)

**Why first:**
- Independently testable against live APIs
- No mock dependencies needed — you can verify correctness immediately
- Provides immediate value even before the other components exist
- Forces resolution of provider-specific edge cases early

**Milestone:** Can run `generate(model="claude-opus-4-6", prompt="Hello")` against all three providers with correct error handling, streaming, and tool calling.

### Phase 2: Coding Agent Loop

**Build second. Depends on the LLM Client.**

The agent loop uses the LLM client as its backend for model calls. It adds the agentic cycle: tools, truncation, loop detection, and session management.

**What to build:**
1. ExecutionEnvironment interface and LocalExecutionEnvironment
2. Tool profiles (provider-specific + core tools)
3. Core agentic loop (call LLM → execute tools → repeat)
4. Output truncation pipeline
5. Loop detection
6. Session configuration and stop conditions
7. Steering and context injection
8. Subagent spawning

**Why second:**
- Needs the LLM client for `CodergenBackend` — the interface for making LLM calls
- Independently testable: create a session, give it a goal, watch it work
- Provides value without the pipeline: a standalone coding agent

**Milestone:** Can run an agent session that reads files, makes edits, runs tests, and iterates until tests pass.

### Phase 3: Attractor Pipeline

**Build last. Depends on both.**

The pipeline orchestrator manages the graph, state, and transitions. It creates agent sessions (from Phase 2) that use the LLM client (from Phase 1).

**What to build:**
1. DOT parser (subset) and graph AST
2. Validation rules
3. Handler registry and built-in handlers
4. Execution engine (traversal loop)
5. Context store and checkpoint system
6. Edge selection and condition evaluation
7. Goal gates
8. Parallel execution (parallel/fan-in handlers)
9. Human-in-the-loop (Interviewer interface)
10. Model stylesheet
11. Observability (events, HTTP server)

**Why last:**
- Orchestrates the other two components — needs them to exist
- Most complex component with the most moving parts
- Benefits from having the lower layers stable before building on top

**Milestone:** Can load a DOT file, execute a multi-node pipeline with codergen, conditional routing, and goal gates, producing a working codebase.

## Incremental Value

Each phase delivers standalone value:

| Phase | Standalone Use Case |
|-------|-------------------|
| LLM Client | Any application needing multi-provider LLM access |
| Agent Loop | Interactive or scripted coding agent sessions |
| Pipeline | Automated multi-stage coding workflows |

You don't need all three to get value. The LLM client alone is useful for any LLM-powered application. The agent loop alone is a coding assistant. The pipeline is the full orchestration vision.

## See Also

- [../00-overview/architecture-overview.md](../00-overview/architecture-overview.md) — The dependency chain
- [decision-points.md](decision-points.md) — Decisions to make at each phase
- [definition-of-done.md](definition-of-done.md) — What "complete" looks like per phase
