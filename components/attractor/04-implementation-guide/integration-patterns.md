# Integration Patterns

How the three components connect at their interfaces.

## Primary Integration: Pipeline → Agent Loop

The pipeline's `codergen` handler (default node type) creates an agent session:

```
Pipeline Node                    Agent Loop Session
─────────────                    ──────────────────
goal attribute        →          SessionConfig.goal
model stylesheet      →          SessionConfig.model, .provider
context store         →          SessionConfig.context (initial state)
fidelity mode         →          System prompt construction
execution environment →          SessionConfig.environment

                                 Session runs...

SessionConfig.goal    ←          Outcome.status
context_updates       ←          Outcome.context_updates
artifacts             ←          Outcome.artifacts
```

### CodergenBackend Interface

The pipeline doesn't directly create `Session` objects — it uses a `CodergenBackend` interface:

```python
class CodergenBackend:
    def create_session(self, config: SessionConfig) -> Session:
        """Create a new agent session with the given config."""
        ...

    def run_session(self, session: Session) -> Outcome:
        """Run the session to completion and return the outcome."""
        ...
```

This abstraction allows:
- Different backend implementations (local, remote, mocked)
- Testing the pipeline without real LLM calls
- Swapping the agent loop implementation

## Primary Integration: Agent Loop → LLM Client

The agent loop uses the LLM client for every model call:

```
Agent Loop                       LLM Client
──────────                       ──────────
System prompt + history   →      Request.messages
Tool definitions          →      Request.tools
Model + provider          →      Request.model, .provider

                                 Client.complete() or .stream()

Tool calls                ←      Response.content (TOOL_CALL parts)
Text response             ←      Response.content (TEXT parts)
Usage                     ←      Response.usage
```

### Session's LLM Backend

The agent loop uses whatever client is configured:

```python
# The session holds a reference to the LLM client
session = Session(
    config=config,
    llm_client=Client.from_env(),  # Or any Client instance
    environment=LocalExecutionEnvironment(...)
)
```

## Context Fidelity → Session Management

Pipeline context fidelity modes map to how much history the agent session receives:

| Fidelity Mode | Agent Session History |
|---------------|----------------------|
| `full` | Complete conversation from prior nodes' sessions |
| `compact` | System prompt + prior node's final output + current goal |
| `summary:low` | Brief summary of prior work |
| `summary:medium` | Moderate summary with key decisions and outcomes |
| `summary:high` | Detailed summary preserving code references |

The pipeline's context fidelity engine prepares the history before creating the agent session. The session itself doesn't know about fidelity — it just receives messages.

## Parallel Handler → Subagent Spawning

Two levels of concurrency that interact:

| Level | Mechanism | Who Decides |
|-------|-----------|-------------|
| **Pipeline parallel** | `hexagon` → multiple graph branches | The graph structure |
| **Agent subagents** | `spawn_subagent` tool | The model during execution |

Pipeline parallelism creates independent agent sessions on different branches. Within each session, the model may additionally spawn subagents.

The execution environment is shared across all sessions in a pipeline run (they work on the same filesystem). Pipeline branches get cloned contexts but share the filesystem.

## Model Stylesheet → Client Configuration

The model stylesheet resolves to per-node model/provider assignments:

```
Model Stylesheet                 LLM Client Request
────────────────                 ──────────────────
#node_name { llm_model: X }  →  Request(model="X")
.class { llm_provider: Y }  →  Request(provider="Y")
* { reasoning_effort: Z }   →  Request(reasoning_effort="Z")
```

The stylesheet is resolved during pipeline initialization (Phase 3). By the time a node executes, its model/provider are determined.

## Error Flow

Errors propagate upward through the stack:

```
LLM API Error (e.g., 429 rate limit)
    ↓ Handled by LLM Client retry logic
    ↓ If retries exhausted → SDKError raised
    ↓
Agent Loop catches SDKError
    ↓ May retry the turn or fail the session
    ↓ If session fails → Outcome(status=FAILURE)
    ↓
Pipeline receives FAILURE outcome
    ↓ Edge selection evaluates conditions
    ↓ Failure routing: fail edge → retry_target → fallback → terminate
```

Each layer handles what it can and escalates what it can't.

## See Also

- [build-sequence.md](build-sequence.md) — The build order these integrations require
- [decision-points.md](decision-points.md) — Decisions that affect integration design
- [../00-overview/architecture-overview.md](../00-overview/architecture-overview.md) — Visual overview of the stack
- [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md) — Context fidelity details
- [../02-coding-agent-loop/agentic-loop.md](../02-coding-agent-loop/agentic-loop.md) — The agent loop internals
