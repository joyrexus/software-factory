# Tool Calling

The client supports a unified tool calling protocol across all providers, including parallel tool execution and automatic tool loops.

## Tool Definition

```python
class Tool:
    name: str                    # Function name
    description: str             # What the tool does (sent to the model)
    parameters: dict             # JSON Schema for arguments
    execute: callable = None     # Handler function (optional — see active vs passive)
```

The parameters use **JSON Schema** as the canonical format — it's universal across all providers. Language-native helpers can generate JSON Schema, but JSON Schema is what gets sent to the API.

## Active vs Passive Tools

| Type | Has `execute` handler | Behavior |
|------|----------------------|----------|
| **Active** | Yes | Client executes the tool automatically and loops |
| **Passive** | No | Client returns tool calls to the caller without executing |

Active tools enable automatic agentic loops — the client calls the model, the model requests a tool, the client executes it, sends the result back, and repeats. This is the pattern used by the [Coding Agent Loop](../02-coding-agent-loop/INDEX.md).

Passive tools are for callers that want to handle tool execution themselves.

## Parallel Tool Execution

When a model returns multiple tool calls in a single response, the correct behavior is:

1. Execute **all** tool calls concurrently
2. Wait for **all** to complete
3. Send **all** results back in a single continuation request

The spec explicitly handles this in the SDK rather than leaving it to callers:

> When a model returns 5 parallel tool calls, the correct behavior is to execute all 5 concurrently, wait for all to complete, and send all 5 results back in one continuation. This is fiddly to implement correctly and identical for every consumer.

## Tool Choice Modes

Control whether and how the model uses tools:

| Mode | Description | Provider Translation |
|------|-------------|---------------------|
| `auto` | Model decides whether to use tools | Default for all providers |
| `none` | Model cannot use tools | OpenAI: `"none"`, Anthropic: omit tools, Gemini: `"NONE"` |
| `required` | Model must use at least one tool | Provider-specific enforcement |
| `named(name)` | Model must call the specified tool | Forces specific tool selection |

## Tool Execution Error Handling

When a tool execution fails:

```python
# Error results are sent to the model, NOT raised as exceptions
ToolResultData(
    tool_call_id="call_123",
    content="Error: FileNotFoundError — src/missing.py does not exist",
    is_error=True
)
```

This gives the model the opportunity to retry, use a different tool, or explain the failure. Raising an exception would abort the entire generation.

Unknown tool calls (model requests a tool not in the definitions) also receive an error result:

```
Error: Unknown tool "nonexistent_tool". Available tools: read_file, edit_file, run_command
```

## Max Tool Rounds

The `max_tool_rounds` parameter limits how many tool call → result → continue cycles occur:

```python
result = generate(model="...", tools=[...], max_tool_rounds=5)
```

| Value | Behavior |
|-------|----------|
| 0 | No automatic execution (passive mode even for active tools) |
| 1 | Default. One round of tool calls. |
| N | Up to N rounds. |

## StepResult

Each tool round produces a `StepResult` tracking what happened:

```python
class StepResult:
    tool_calls: list[ToolCallData]     # What the model requested
    tool_results: list[ToolResultData]  # What came back
    usage: Usage                        # Token usage for this step
```

The final `GenerateResult` includes a list of all `StepResult` objects and aggregated usage.

## See Also

- [messages-and-content.md](messages-and-content.md) — ToolCallData and ToolResultData content parts
- [high-level-api.md](high-level-api.md) — generate() with tools
- [streaming.md](streaming.md) — Streaming with tool calls
- [provider-adapters.md](provider-adapters.md) — Per-provider tool call format differences
- [../02-coding-agent-loop/agentic-loop.md](../02-coding-agent-loop/agentic-loop.md) — How the agent loop uses active tools
