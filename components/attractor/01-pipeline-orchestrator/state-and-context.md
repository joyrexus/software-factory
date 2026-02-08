# State and Context

The pipeline's shared state is a key-value context store that persists across node executions. Handlers read inputs from context and write outputs back as context updates.

## Context Store

The context is a flat key-value map with string keys and typed values (strings, numbers, booleans, lists, maps). It serves as the communication channel between nodes.

```python
# Reading from context
feature_name = context.get("feature_name")
test_results = context.get("test_results")

# Writing to context (via outcome)
outcome = Outcome(
    status=Status.SUCCESS,
    context_updates={"module_path": "src/auth.py", "tests_passing": True}
)
```

## Thread Safety

The context store must be thread-safe for parallel execution. During parallel branches, each branch operates on a **cloned copy** of the context. At the fan-in point, branch contexts are merged according to the join policy (see [parallelism.md](parallelism.md)).

## Built-In Keys

The engine maintains several reserved keys:

| Key | Type | Description |
|-----|------|-------------|
| `_current_node` | string | ID of the currently executing node |
| `_previous_node` | string | ID of the previously executed node |
| `_pipeline_name` | string | Name from the digraph declaration |
| `_run_id` | string | Unique identifier for this execution run |
| `_retry_count` | integer | Number of retries for the current node |
| `_start_time` | timestamp | When the pipeline run began |
| `_node_start_time` | timestamp | When the current node began execution |

Built-in keys use the `_` prefix. User-defined keys should avoid this prefix.

## Namespace Conventions

For complex pipelines, use dotted namespaces to organize context keys:

```
auth.module_path = "src/auth.py"
auth.tests_passing = true
validation.coverage = 0.87
validation.lint_errors = 0
```

This is a convention, not enforcement — the context store treats dotted keys as flat strings.

## Outcome Model

Every handler returns an `Outcome` describing what happened:

```python
class Outcome:
    status: Status           # SUCCESS, FAILURE, PARTIAL, SKIPPED
    context_updates: dict    # Key-value pairs to merge into context
    preferred_label: str     # Preferred outgoing edge label (optional)
    suggested_next: list     # Suggested next node IDs (optional)
    artifacts: list          # References to produced artifacts (optional)
    error: ErrorInfo         # Error details if status is FAILURE (optional)
    metadata: dict           # Handler-specific metadata (optional)
```

### Status Enum

| Status | Meaning |
|--------|---------|
| `SUCCESS` | Node completed its task successfully |
| `FAILURE` | Node failed (may be retryable) |
| `PARTIAL` | Node completed some but not all work |
| `SKIPPED` | Node was intentionally skipped |

### Error Info

When `status` is `FAILURE`, the `error` field provides details:

```python
class ErrorInfo:
    type: str          # Error category (e.g., "TERMINAL", "TRANSIENT", "TIMEOUT")
    message: str       # Human-readable error description
    retryable: bool    # Whether the engine should attempt retry
    details: dict      # Additional error context
```

## Context Fidelity

When a codergen node executes, the engine creates an agent session. Context fidelity controls how much prior conversation history is carried into that session:

| Mode | What's Included | Token Cost | Use Case |
|------|-----------------|------------|----------|
| `full` | Complete conversation history from all prior nodes | High | Critical nodes needing full context |
| `compact` | System prompt + last node's output + current goal | Medium | Default for most transitions |
| `summary:low` | Brief summary of prior work | Low | Long pipelines where full context exceeds limits |
| `summary:medium` | Moderate summary with key details | Medium | Balance of context and cost |
| `summary:high` | Detailed summary preserving structure | Medium-High | When details matter but full history is too large |

Fidelity can be set at three levels (in override order):
1. **Edge-level**: `fidelity` attribute on a specific edge
2. **Node-level**: `fidelity` attribute on the target node
3. **Graph-level**: `default_fidelity` graph attribute

Edge-level overrides node-level, which overrides graph-level.

## Context and Agent Sessions

The mapping between pipeline context and agent sessions:

- **Pipeline context** → Agent session's initial context / system prompt content
- **Agent session output** → Pipeline outcome context_updates
- **Context fidelity** → Controls how much of the agent session's history carries forward

See [../04-implementation-guide/integration-patterns.md](../04-implementation-guide/integration-patterns.md) for the full integration mapping.

## See Also

- [execution-engine.md](execution-engine.md) — How context is updated during traversal
- [checkpoints-and-resume.md](checkpoints-and-resume.md) — How context is persisted
- [parallelism.md](parallelism.md) — Context cloning and merging for parallel branches
- [edge-routing-and-conditions.md](edge-routing-and-conditions.md) — How conditions evaluate against outcomes
