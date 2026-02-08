# Session and Config

Each agent session is configured via a `SessionConfig` that controls behavior, limits, and model parameters.

## SessionConfig Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `goal` | string | required | The task description for the agent |
| `system_prompt` | string | (built-in) | Override the default system prompt |
| `model` | string | (from stylesheet) | LLM model identifier |
| `provider` | string | (from stylesheet) | LLM provider name |
| `max_rounds` | integer | 10 | Maximum loop iterations |
| `max_turns` | integer | 50 | Hard limit on LLM calls |
| `reasoning_effort` | string | `medium` | Reasoning intensity (low/medium/high) |
| `temperature` | float | 0.0 | Sampling temperature |
| `max_tokens` | integer | 4096 | Maximum output tokens per LLM call |
| `timeout` | integer | 600 | Session timeout in seconds |
| `environment` | ExecutionEnvironment | Local | Where tools execute |
| `additional_tools` | list | [] | Extra tools beyond the provider profile |
| `steering` | list | [] | Initial steering messages |
| `follow_ups` | list | [] | Messages to inject after each turn |
| `context` | dict | {} | Initial key-value context |

## Stop Conditions

The session ends when any of these conditions is met:

| Condition | Description | Config Field |
|-----------|-------------|--------------|
| **Natural completion** | Model responds without tool calls | (always active) |
| **Round limit** | Maximum rounds reached | `max_rounds` |
| **Turn limit** | Maximum turns reached | `max_turns` |
| **Abort** | External cancellation signal | (runtime) |
| **Error** | Unrecoverable error | (runtime) |
| **Timeout** | Session wall-clock time exceeded | `timeout` |

### Round Limit vs Turn Limit

- `max_rounds` is the primary safety net. A "round" is one complete cycle of the agent doing meaningful work.
- `max_turns` is the hard ceiling on total LLM calls, regardless of what constitutes a "round."
- In practice, `max_turns` should be set significantly higher than `max_rounds` as a backstop.

## Reasoning Effort

Controls how much "thinking" the model does:

| Level | Effect | Use Case |
|-------|--------|----------|
| `low` | Faster, less thorough | Simple file edits, boilerplate generation |
| `medium` | Balanced (default) | Most coding tasks |
| `high` | Slower, more thorough | Complex architecture decisions, debugging |

For providers that support reasoning tokens (OpenAI's reasoning models, Anthropic's extended thinking), this maps to their native reasoning controls. For others, it may influence temperature or prompt structure.

## Session Lifecycle

```
config = SessionConfig(goal="Implement user authentication")
session = Session(config, environment=LocalExecutionEnvironment(...))
outcome = session.run()

# outcome.status: COMPLETED, FAILED, ROUND_LIMIT, etc.
# outcome.artifacts: files created/modified
# outcome.context_updates: key-value pairs for the pipeline
```

When created by an Attractor pipeline, the session config is populated from:
- The node's `goal` attribute → `config.goal`
- The model stylesheet → `config.model`, `config.provider`
- The pipeline context → `config.context`
- The node's fidelity mode → system prompt construction

## See Also

- [agentic-loop.md](agentic-loop.md) — The loop that uses this config
- [steering-and-context.md](steering-and-context.md) — Steering and follow-up queues
- [execution-environment.md](execution-environment.md) — The environment field
- [../01-pipeline-orchestrator/model-stylesheet.md](../01-pipeline-orchestrator/model-stylesheet.md) — How model/provider are determined
- [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md) — Context fidelity modes
