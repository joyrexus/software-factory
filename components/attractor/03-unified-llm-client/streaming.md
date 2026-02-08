# Streaming

The client supports streaming responses via Server-Sent Events (SSE), with a unified event model across providers.

## Stream Event Types

| Event Type | Description |
|------------|-------------|
| `STREAM_START` | Stream has begun, includes request metadata |
| `TEXT_DELTA` | Incremental text content |
| `TOOL_CALL_START` | Beginning of a tool call (includes tool name) |
| `TOOL_CALL_DELTA` | Incremental tool call arguments |
| `TOOL_CALL_END` | Tool call complete |
| `THINKING_DELTA` | Incremental thinking/reasoning content |
| `USAGE` | Token usage information |
| `FINISH` | Stream complete, includes finish reason |

## Start/Delta/End Pattern

Structured content follows a start → delta → end pattern:

```
STREAM_START
TEXT_DELTA "Here is "
TEXT_DELTA "the answer"
TOOL_CALL_START { name: "read_file" }
TOOL_CALL_DELTA { arguments_chunk: '{"path":' }
TOOL_CALL_DELTA { arguments_chunk: '"src/main.py"}' }
TOOL_CALL_END { id: "call_123" }
USAGE { input: 100, output: 50 }
FINISH { reason: "tool_calls" }
```

This pattern preserves structural information when a response contains multiple text segments or interleaved tool calls. Flat deltas would lose this structure.

## Why Separate Start/Delta/End

> Flat deltas lose structural information when a response contains multiple text segments or interleaved tool calls. The pattern adds minimal overhead but enables correct handling of complex responses.

Without structure:
- You can't tell when one text segment ends and another begins
- Interleaved tool calls and text become ambiguous
- Building a complete response from deltas requires heuristics

## StreamAccumulator

A utility that collects stream events and builds a complete response:

```python
stream = client.stream(request)
accumulator = StreamAccumulator()

for event in stream:
    accumulator.add(event)
    if event.type == "TEXT_DELTA":
        print(event.delta, end="")  # Real-time output

response = accumulator.response()  # Complete Response object
```

The accumulator handles:
- Concatenating text deltas into complete text
- Assembling tool call arguments from deltas
- Aggregating usage across events
- Building the final `Response` equivalent

## Provider Streaming Differences

Each provider has a different SSE format:

| Provider | Format | Termination Signal |
|----------|--------|-------------------|
| OpenAI | SSE with `data:` lines | `data: [DONE]` |
| Anthropic | SSE with event type + data lines | `message_stop` event |
| Gemini | SSE (with `?alt=sse`) or JSON | No explicit signal (final chunk) |

The adapters translate these to the unified `StreamEvent` types.

## Streaming with Tool Calls

When a streamed response includes tool calls:

1. Stream events arrive: `TOOL_CALL_START` → `TOOL_CALL_DELTA` → `TOOL_CALL_END`
2. After `FINISH` (with reason `tool_calls`), the accumulated tool calls are available
3. If using active tools, the client executes them and starts a new stream
4. The new stream's events continue from where the first stream ended

## Cancellation

Streams can be cancelled mid-flight:

```python
stream = client.stream(request)
for event in stream:
    if should_stop(event):
        stream.cancel()  # Stops reading from the provider
        break
```

Cancellation is cooperative — it closes the SSE connection but any data already received is valid.

## See Also

- [four-layer-architecture.md](four-layer-architecture.md) — Streaming-first as a design principle
- [provider-adapters.md](provider-adapters.md) — Per-provider streaming translation
- [high-level-api.md](high-level-api.md) — The stream() convenience function
- [tool-calling.md](tool-calling.md) — Tool calls in streamed responses
