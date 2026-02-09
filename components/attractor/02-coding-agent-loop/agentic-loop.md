# Agentic Loop

The core loop that drives coding agent sessions: send a prompt, receive tool calls, execute tools, send results back, repeat.

## The Loop

```
┌─────────────┐
│   Input      │  System prompt + goal + context
└──────┬──────┘
       ▼
┌─────────────┐
│   LLM Call   │  Send messages to model via Unified LLM Client
└──────┬──────┘
       ▼
┌─────────────┐     ┌─────────────┐
│  Tool Calls? │──▶  │  Execute     │  Run tools in execution environment
└──────┬──────┘     │  Tools       │
       │            └──────┬──────┘
       │                   │
       │            ┌──────▼──────┐
       │            │  Truncate    │  Trim output if needed
       │            │  Output      │
       │            └──────┬──────┘
       │                   │
       │            ┌──────▼──────┐
       │            │  Loop        │  Check for repetitive patterns
       │            │  Detection   │
       │            └──────┬──────┘
       │                   │
       │                   ▼
       │            ┌─────────────┐
       │            │  Send Tool   │  Return results to model
       │            │  Results     │──────────┐
       │            └─────────────┘          │
       │                                      │
       ▼                                      ▼
┌─────────────┐                      Back to LLM Call
│  Completion  │  No more tool calls = task done
│  Check       │
└─────────────┘
```

Each iteration through the loop is a **turn**. Multiple tool executions within a single LLM response count as one turn.

## Session State Machine

A session progresses through these states:

```
CREATED → RUNNING → { COMPLETED | FAILED | ABORTED | ROUND_LIMIT | TURN_LIMIT }
```

| State | Meaning |
|-------|---------|
| `CREATED` | Session initialized, not yet started |
| `RUNNING` | Loop is actively executing |
| `COMPLETED` | Model signaled natural completion (no more tool calls) |
| `FAILED` | Unrecoverable error during execution |
| `ABORTED` | External abort signal received |
| `ROUND_LIMIT` | Maximum round count reached |
| `TURN_LIMIT` | Maximum turn count reached |

## Completion Detection

The loop ends when any of these conditions is met:

1. **Natural completion**: The model responds with text only (no tool calls) — it considers the task done
2. **Round limit**: `max_rounds` reached (configurable per session)
3. **Turn limit**: `max_turns` reached
4. **Abort signal**: External cancellation
5. **Fatal error**: An error that can't be recovered from (e.g., authentication failure)

"Natural completion" is the expected path — the model decides when it's done. The limits are safety nets.

## What Happens Each Turn

1. **Assemble messages**: System prompt + conversation history + any steering/follow-up injections
2. **Call LLM**: Via `Client.complete()` or `Client.stream()` from the [Unified LLM Client](../03-unified-llm-client/README.md)
3. **Process response**: If the model returns tool calls, execute them all (potentially in parallel)
4. **Truncate outputs**: Apply the truncation pipeline to tool outputs that exceed limits
5. **Check for loops**: Compare recent tool call signatures against history
6. **Inject warnings**: If a loop is detected, inject a warning message into the conversation
7. **Append to history**: Add the assistant message and tool results to conversation history
8. **Check stop conditions**: Evaluate whether to continue or stop

## Rounds vs Turns

- A **turn** is one LLM call → tool execution → results cycle
- A **round** is a higher-level iteration that may encompass multiple turns (implementation-defined)
- `max_rounds` is typically the primary stop condition
- `max_turns` is a hard safety limit

## See Also

- [tools-and-truncation.md](tools-and-truncation.md) — Step 4: output truncation
- [loop-detection.md](loop-detection.md) — Steps 5-6: detecting repetitive patterns
- [session-and-config.md](session-and-config.md) — Stop conditions and limits
- [steering-and-context.md](steering-and-context.md) — How steering injections work at step 1
- [../03-unified-llm-client/tool-calling.md](../03-unified-llm-client/tool-calling.md) — The tool calling protocol
