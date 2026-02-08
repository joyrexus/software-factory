# Decision Points

Key decisions your organization needs to make before and during implementation. These are places where the spec is intentionally flexible.

## LLM Client Decisions

### Which providers to support first?

The spec covers OpenAI, Anthropic, and Gemini. You don't need all three on day one.

| Option | Rationale |
|--------|-----------|
| **Anthropic first** | Best tool use performance for coding tasks; native thinking blocks |
| **OpenAI first** | Widest ecosystem; reasoning tokens via Responses API |
| **All three in parallel** | Maximum flexibility from the start; higher initial effort |

**Recommendation**: Start with Anthropic (primary coding model) + OpenAI (fallback/comparison), add Gemini later.

### OpenAI-compatible adapter priority?

Many teams use vLLM, Ollama, or Together AI. Building the `OpenAICompatibleAdapter` unlocks these.

**Decision**: Is local/self-hosted inference a near-term need? If yes, prioritize the compatible adapter alongside the primary OpenAI adapter.

## Agent Loop Decisions

### Execution environment strategy

The spec defines the `ExecutionEnvironment` interface but only specifies `LocalExecutionEnvironment`. Which environments do you need?

| Environment | When to Prioritize |
|-------------|-------------------|
| **Local only** | MVP, developer workstations, trusted code |
| **Docker** | CI/CD pipelines, untrusted code, reproducibility |
| **Kubernetes** | Production workloads, scaling, cloud-native |
| **WASM** | Lightweight sandboxing, browser-based tools |

**Recommendation**: Start with Local. Add Docker when you need isolation. Others as scaling requires.

### Tool set customization

The spec defines core tools + provider profiles. Do you need additional custom tools?

Common additions:
- Database query tools
- API interaction tools
- Deployment/CI tools
- Code analysis tools (linting, type checking)

## Pipeline Decisions

### Checkpoint storage backend

The spec is agnostic. Options:

| Backend | Pros | Cons |
|---------|------|------|
| **Local filesystem (JSON)** | Simple, no dependencies | No distributed access |
| **SQLite** | Structured queries, still local | Single-machine |
| **PostgreSQL/MySQL** | Distributed, queryable | Infrastructure dependency |
| **S3/GCS** | Cloud-native, durable | Latency, eventual consistency |

**Recommendation**: Start with local filesystem. Move to a database when you need distributed execution or queryable history.

### Human-in-the-loop UX

The Interviewer interface is abstract. What's your first implementation?

| Implementation | When |
|----------------|------|
| **ConsoleInterviewer** | CLI tools, developer workflows |
| **AutoApproveInterviewer** | Testing, CI/CD (no human available) |
| **Slack bot** | Team workflows, async approvals |
| **Web UI** | Dashboard-based operations |

**Recommendation**: Console for development, AutoApprove for CI. Add Slack or web UI when non-engineers need to interact.

### HTTP API exposure

The spec describes an optional HTTP server mode. When do you need it?

- **Skip initially**: If pipelines are invoked programmatically or from CLI
- **Build early**: If you need a web UI, remote monitoring, or multi-user access
- **Consider API gateway**: If you need auth, rate limiting, or multi-tenant isolation

### Testing strategy per layer

| Layer | Testing Approach |
|-------|-----------------|
| **LLM Client** | Integration tests against live APIs + mock adapters for unit tests |
| **Agent Loop** | End-to-end tests with real models on small tasks + mocked environment for unit tests |
| **Pipeline** | Pipeline definition tests (validation) + small end-to-end pipelines with real agent sessions |

**Key question**: How do you test without spending on API calls? The spec's AutoApproveInterviewer and QueueInterviewer patterns help with deterministic replay.

## Cross-Cutting Decisions

### Implementation language

The specs are language-agnostic. Key considerations:

| Language | Strengths for This System |
|----------|--------------------------|
| **Python** | Rich LLM ecosystem, fastest to prototype, most examples in specs |
| **Go** | Performance, concurrency primitives, deployment simplicity |
| **TypeScript** | Full-stack potential, streaming SSE natural fit |

### Monorepo vs multi-repo

The three components are independent libraries with clear interfaces:

| Approach | When |
|----------|------|
| **Monorepo** | Small team, rapid iteration, shared tooling |
| **Multi-repo** | Independent release cycles, different teams per component |

### Observability infrastructure

The spec emits events but doesn't prescribe how to consume them:

| Option | Complexity |
|--------|-----------|
| **Log files only** | Minimal — just write events to disk |
| **Structured logging (JSON)** | Low — ship to any log aggregator |
| **OpenTelemetry** | Medium — distributed tracing, standard format |
| **Custom dashboard** | High — real-time monitoring, analytics |

## See Also

- [build-sequence.md](build-sequence.md) — When each decision needs to be made
- [integration-patterns.md](integration-patterns.md) — How decisions affect integration points
- [../06-comparisons/ramp-inspect.md](../06-comparisons/ramp-inspect.md) — How Ramp made similar decisions
