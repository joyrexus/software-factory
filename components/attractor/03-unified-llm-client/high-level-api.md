# High-Level API

Layer 4 of the architecture — convenience functions that wrap the core client for common use cases.

## generate()

Simple prompt-in, response-out:

```python
result = generate(
    model="claude-opus-4-6",
    prompt="Explain quantum computing",
    system="You are a helpful assistant.",
    max_tokens=1000
)

print(result.text)           # The response text
print(result.usage)          # Token usage
print(result.finish_reason)  # Why generation stopped
```

### With Tools

```python
result = generate(
    model="claude-opus-4-6",
    prompt="What is the weather in San Francisco?",
    tools=[weather_tool],
    max_tool_rounds=5
)

print(result.text)                    # Final text after all tool rounds
print(len(result.steps))             # Number of tool round-trips
print(result.total_usage.total_tokens)  # Aggregated tokens across all steps
```

### With Messages (Instead of Prompt)

```python
result = generate(
    model="claude-opus-4-6",
    messages=[
        Message(role=SYSTEM, content=[ContentPart(kind=TEXT, text="You are a math tutor.")]),
        Message(role=USER, content=[ContentPart(kind=TEXT, text="What is 2+2?")])
    ]
)
```

`prompt` and `messages` are mutually exclusive — providing both raises an error.

## stream()

Streaming response with real-time output:

```python
result = stream(model="claude-opus-4-6", prompt="Write a poem")

for event in result:
    if event.type == "TEXT_DELTA":
        print(event.delta, end="")

response = result.response()  # Complete response after stream ends
print(response.usage)
```

## generate_object()

Structured output with JSON schema validation:

```python
result = generate_object(
    model="gpt-5.2",
    prompt="Extract: Alice is 30 years old",
    schema={
        "type": "object",
        "properties": {
            "name": {"type": "string"},
            "age": {"type": "integer"}
        },
        "required": ["name", "age"]
    }
)

print(result.output)  # {"name": "Alice", "age": 30}
```

If the model's output can't be parsed or validated against the schema, raises `NoObjectGeneratedError`.

Provider behavior varies:
- **OpenAI**: Native `json_schema` response format
- **Anthropic**: Prompt engineering or tool-based extraction
- **Gemini**: Native `responseSchema`

## Return Types

| Function | Returns | Key Fields |
|----------|---------|------------|
| `generate()` | `GenerateResult` | `text`, `usage`, `finish_reason`, `steps`, `total_usage` |
| `stream()` | `StreamResult` (iterable) | Events; `.response()` for complete result |
| `generate_object()` | `ObjectResult` | `output` (parsed object), `text`, `usage` |

## Usage Aggregation

For multi-step operations (tool loops), usage is tracked per-step and aggregated:

```python
result = generate(model="...", tools=[...], max_tool_rounds=5)

# Per-step usage
for step in result.steps:
    print(f"Step: {step.usage.input_tokens} in, {step.usage.output_tokens} out")

# Aggregated usage
print(f"Total: {result.total_usage.total_tokens} tokens")
```

## Design Decision: Separate generate() and stream()

> The return types are fundamentally different: GenerateResult vs StreamResult. A boolean flag loses type safety.

A single function with `stream=True/False` would return different types based on a runtime flag, losing the type system's ability to check correct usage at compile time.

## See Also

- [four-layer-architecture.md](four-layer-architecture.md) — High-level API as Layer 4
- [client-setup.md](client-setup.md) — Default client for module-level functions
- [tool-calling.md](tool-calling.md) — How tools work with generate()
- [streaming.md](streaming.md) — Stream event model
- [messages-and-content.md](messages-and-content.md) — Message format for the messages parameter
