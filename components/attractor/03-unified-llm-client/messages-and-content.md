# Messages and Content

The unified message model provides a single representation for conversations across all providers.

## Message Record

```python
class Message:
    role: Role                    # Who sent this message
    content: list[ContentPart]    # One or more content parts
    tool_call_id: str = None      # For TOOL role: which tool call this responds to
```

Messages are the fundamental unit of conversation. A conversation is a list of messages.

## Role Enum

| Role | Description |
|------|-------------|
| `SYSTEM` | System instructions (sets behavior) |
| `USER` | Human or application input |
| `ASSISTANT` | Model output |
| `TOOL` | Tool execution result |
| `DEVELOPER` | Developer-level instructions (higher priority than system for some providers) |

### Provider Role Translation

Not all providers handle all roles the same way:

| Role | OpenAI | Anthropic | Gemini |
|------|--------|-----------|--------|
| SYSTEM | `instructions` parameter | Extracted to `system` parameter | Extracted to `systemInstruction` |
| DEVELOPER | `developer` role | Merged with system | Merged with system |
| USER | `user` role | `user` role (strict alternation required) | `user` role |
| ASSISTANT | `assistant` role | `assistant` role | `model` role |
| TOOL | `tool` role | `tool_result` blocks in user message | `functionResponse` in user content |

Note: Anthropic requires **strict user/assistant alternation**. The adapter handles merging consecutive same-role messages.

## ContentPart Tagged Union

Each content part has a `kind` that determines its data:

| Kind | Data Fields | Description |
|------|-------------|-------------|
| `TEXT` | `text: str` | Plain text content |
| `IMAGE` | `image: ImageData` | Image (URL or base64 data) |
| `AUDIO` | `audio: AudioData` | Audio content |
| `TOOL_CALL` | `tool_call: ToolCallData` | Model requesting a tool execution |
| `TOOL_RESULT` | `tool_result: ToolResultData` | Result from a tool execution |
| `THINKING` | `thinking: ThinkingData` | Model's reasoning process (Anthropic) |
| `REDACTED_THINKING` | (opaque) | Redacted thinking block (pass-through) |
| `DOCUMENT` | `document: DocumentData` | Document content (PDF, etc.) |

### ImageData

```python
class ImageData:
    url: str = None           # Image URL
    data: bytes = None        # Base64-encoded image data
    media_type: str = None    # MIME type (e.g., "image/png")
```

Images can be provided as URLs or inline base64 data. The adapter translates to the provider's expected format:
- OpenAI: `image_url` data URI
- Anthropic: `base64` source with `media_type`
- Gemini: `inlineData` with `mimeType`

### ToolCallData

```python
class ToolCallData:
    id: str            # Unique identifier for this call
    name: str          # Tool function name
    arguments: dict    # Parsed arguments
```

### ToolResultData

```python
class ToolResultData:
    tool_call_id: str    # Matches the ToolCallData.id
    content: str         # Result content
    is_error: bool       # Whether the tool execution failed
```

### ThinkingData

```python
class ThinkingData:
    text: str            # The model's reasoning text
    signature: str       # Cryptographic signature for round-tripping
```

Thinking blocks must be preserved in conversation history with their signatures intact, so the provider can verify their integrity on subsequent calls.

## Convenience Constructors

For common cases:

```python
# Text-only message
msg = Message.user("What is 2 + 2?")

# Multimodal message
msg = Message.user([
    ContentPart(kind=TEXT, text="What do you see?"),
    ContentPart(kind=IMAGE, image=ImageData(url="https://example.com/photo.jpg"))
])
```

## See Also

- [tool-calling.md](tool-calling.md) — How tool calls and results work in conversations
- [streaming.md](streaming.md) — How content parts arrive incrementally
- [provider-adapters.md](provider-adapters.md) — How messages translate to each provider
