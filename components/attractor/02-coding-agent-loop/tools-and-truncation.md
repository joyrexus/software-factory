# Tools and Truncation

Tool outputs can be large — test suite results, file contents, command output. The truncation pipeline ensures outputs stay within LLM context limits while preserving the most useful information.

## The Problem

When a tool returns, say, 50,000 characters of test output, sending all of it to the LLM:
- Wastes tokens (and money)
- May exceed context window limits
- Dilutes the signal with noise (the LLM needs the failure messages, not 500 lines of passing tests)

## Truncation Pipeline

Tool output goes through a two-stage pipeline:

### Stage 1: Character-Based Truncation

If total output exceeds the character limit, truncate to that limit:

```
Output: "...50,000 characters..."
Limit:  10,000 characters
Result: First 10,000 characters (or head/tail split — see below)
```

### Stage 2: Line-Based Truncation

If the character-truncated output exceeds the line limit, apply line truncation:

```
Output: 500 lines
Limit:  100 lines
Result: First 30 lines + "... [370 lines truncated] ..." + Last 70 lines
```

## Per-Tool Limits

Different tools have different truncation limits:

| Tool | Character Limit | Line Limit | Rationale |
|------|----------------|------------|-----------|
| `read_file` | 50,000 | 500 | Files can be large but usually have focused regions |
| `run_command` | 20,000 | 200 | Command output is often verbose but key info is at end |
| `list_directory` | 10,000 | 100 | Directory listings are structured and compact |
| `search_files` | 30,000 | 300 | Search results need enough context to be useful |

These are defaults — implementations can adjust per session.

## Head/Tail Split Algorithm

When truncating, the algorithm preserves both the beginning and end of output, since important information often appears in both places (e.g., test summary at the end, setup info at the start):

```python
def truncate(text: str, max_chars: int, head_ratio: float = 0.3) -> str:
    if len(text) <= max_chars:
        return text

    head_size = int(max_chars * head_ratio)
    tail_size = max_chars - head_size

    head = text[:head_size]
    tail = text[-tail_size:]
    omitted = len(text) - head_size - tail_size

    return f"{head}\n\n... [{omitted} characters truncated] ...\n\n{tail}"
```

The `head_ratio` defaults to 0.3 (30% head, 70% tail) because error messages and summaries tend to appear at the end of output.

## Full Output in Events

The truncation pipeline only affects what's sent to the LLM. The **full, untruncated output** is:

- Stored in the event log
- Available in the run directory artifacts
- Accessible via the observability API

This means debugging can always access complete output even though the LLM saw a truncated version.

## Truncation Indicators

When output is truncated, a marker is injected so the LLM knows it's seeing partial output:

```
[First 30 lines of 500]
... output here ...

... [370 lines truncated — full output available in artifacts] ...

[Last 70 lines of 500]
... output here ...
```

This lets the model request the full output (via `read_file` on the artifact) if it needs more context.

## See Also

- [agentic-loop.md](agentic-loop.md) — Where truncation fits in the loop
- [provider-profiles.md](provider-profiles.md) — The tools whose output gets truncated
- [../01-pipeline-orchestrator/observability.md](../01-pipeline-orchestrator/observability.md) — Where full output is stored
