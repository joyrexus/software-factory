# Retry and Failure

When a node fails, the engine follows a structured retry and failure routing system before giving up.

## Retry Configuration

Nodes configure retry behavior through attributes:

```dot
generate [shape=box goal="Write the module" max_retries=3 retry_backoff=exponential]
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `max_retries` | integer | 0 | Maximum number of retry attempts |
| `retry_backoff` | string | `exponential` | Backoff strategy between retries |
| `retry_target` | string | (self) | Node to route to for retry (defaults to re-executing the same node) |
| `retry_jitter` | float | 0.1 | Jitter factor added to backoff delays |

## Backoff Presets

| Preset | Behavior |
|--------|----------|
| `none` | No delay between retries |
| `linear` | Fixed delay (e.g., 5s, 5s, 5s) |
| `exponential` | Doubling delay with jitter (e.g., 1s, 2s, 4s, 8s) |
| `aggressive` | Short initial delay, rapid escalation |

## Retry Predicates

Not all failures should be retried. The engine checks whether a failure is retryable:

| Error Type | Retryable | Rationale |
|------------|-----------|-----------|
| `TRANSIENT` | Yes | Temporary issue (rate limit, network blip) |
| `TIMEOUT` | Configurable | May indicate inherently slow operation |
| `TERMINAL` | No | Fundamental error (invalid input, missing dependency) |
| `UNKNOWN` | Yes (default) | Assume transient unless proven otherwise |

The outcome's `error.retryable` field can override these defaults.

## Failure Routing Priority

When a node fails and retries are exhausted (or the error is non-retryable), the engine follows this routing priority:

| Priority | Mechanism | Description |
|----------|-----------|-------------|
| 1 | **Fail edge** | An outgoing edge with `condition="status == 'FAILURE'"` |
| 2 | **Retry target** | The node specified by `retry_target` attribute |
| 3 | **Fallback** | A designated fallback node (if configured at graph level) |
| 4 | **Terminate** | Pipeline terminates with a FAILURE outcome |

### Fail Edge

The most flexible option — define an explicit edge for failure:

```dot
generate -> fix_errors [condition="status == 'FAILURE'" label="Fix and retry"]
fix_errors -> generate [label="Retry"]
```

This lets you route failures through a repair node before retrying.

### Retry Target

When `retry_target` is set on a node, failures route to that target node instead of re-executing the failed node:

```dot
generate [shape=box goal="Write module" retry_target=fix_errors]
```

### Fallback

A graph-level `fallback` node that catches unhandled failures:

```dot
graph [fallback=error_handler]
error_handler [shape=box goal="Log error and clean up"]
```

### Terminate

If no failure routing is configured, the pipeline terminates with the failure outcome. All goal gates are checked — unsatisfied gates are reported in the terminal status.

## Retry State

During retries:

- The `_retry_count` context key tracks the current attempt number
- Context from previous attempts is preserved (the failed node's partial updates remain)
- Checkpoints are saved before each retry attempt
- The retry counter resets if the node succeeds and is later reached again via a different path

## See Also

- [execution-engine.md](execution-engine.md) — How failure routing fits into the traversal loop
- [goal-gates.md](goal-gates.md) — Goal gate enforcement on pipeline termination
- [checkpoints-and-resume.md](checkpoints-and-resume.md) — Retry counter persistence
- [state-and-context.md](state-and-context.md) — Error types in the Outcome model
