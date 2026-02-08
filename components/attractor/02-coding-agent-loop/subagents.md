# Subagents

An agent session can spawn child sessions (subagents) to handle subtasks independently.

## When to Use Subagents

Subagents are useful when:
- A task naturally decomposes into independent subtasks
- A subtask needs a different model or configuration
- You want to isolate a risky operation (if the subagent fails, the parent continues)
- Parallel exploration of different approaches

## Child Session Spawning

The parent session creates a subagent via a tool call:

```python
# The model calls a "spawn_subagent" tool
result = spawn_subagent(
    goal="Write unit tests for the auth module",
    files=["src/auth.py", "tests/"],
    max_rounds=5
)
```

The subagent runs as a complete, independent session.

## Independent History

Each subagent has its own conversation history:
- The parent's conversation history is **not** shared with the subagent
- The subagent starts fresh with its own system prompt and goal
- Only the final result (outcome + artifacts) flows back to the parent

This isolation prevents the subagent's potentially large conversation from polluting the parent's context window.

## Shared Filesystem

Subagents share the same filesystem (execution environment) as the parent:
- Files written by the parent are visible to the subagent
- Files written by the subagent are visible to the parent (after the subagent completes)
- There's no filesystem isolation — subagents can read and modify any file the parent can

This is by design: the subagent is working on the same project. Filesystem isolation would be provided by the execution environment layer (Docker, WASM, etc.) if needed.

## Depth Limiting

To prevent runaway subagent spawning:

| Config | Default | Description |
|--------|---------|-------------|
| `max_subagent_depth` | 3 | Maximum nesting depth |
| `max_subagents` | 10 | Maximum total subagents per session |

A subagent at depth 3 cannot spawn further subagents. If the depth limit is reached, the `spawn_subagent` tool returns an error telling the model to handle the subtask directly.

## Subagent Tools

Subagents have the same tool set as their parent, except:
- The `spawn_subagent` tool is only available if the depth limit hasn't been reached
- Additional tools can be restricted or added per-subagent via the spawn call

## Subagent Outcome

When a subagent completes, its outcome is returned to the parent as a tool result:

```python
# Tool result seen by the parent model
{
    "status": "SUCCESS",
    "summary": "Created 12 unit tests for auth module. All passing.",
    "files_modified": ["tests/test_auth.py"],
    "artifacts": ["test_results.json"]
}
```

The parent model can then continue with the subagent's results as context.

## Subagents vs Pipeline Parallelism

| Aspect | Subagents | Pipeline Parallel Nodes |
|--------|-----------|------------------------|
| Initiated by | The model (via tool call) | The pipeline graph (via hexagon node) |
| Scope | Within a single node's execution | Across multiple pipeline nodes |
| Context | Independent history, shared filesystem | Cloned context stores |
| Orchestration | Model decides when to spawn | Graph structure determines branches |

Subagents are **model-driven** parallelism (the LLM decides to delegate). Pipeline parallelism is **graph-driven** (the pipeline structure defines concurrent branches).

## See Also

- [agentic-loop.md](agentic-loop.md) — The loop that subagent sessions run
- [session-and-config.md](session-and-config.md) — Configuration inherited by subagents
- [../01-pipeline-orchestrator/parallelism.md](../01-pipeline-orchestrator/parallelism.md) — Graph-level parallelism
