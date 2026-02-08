# Provider Adapters

Each provider adapter translates between the unified interface and a provider's native API. The adapters use each provider's **best** API, not compatibility shims.

## ProviderAdapter Interface

```python
class ProviderAdapter:
    name: str    # Provider identifier (e.g., "anthropic")

    def complete(self, request: Request) -> Response:
        """Send a request and return a complete response."""
        ...

    def stream(self, request: Request) -> Iterator[StreamEvent]:
        """Send a request and return a stream of events."""
        ...
```

## Native APIs

| Provider | Native API | Endpoint |
|----------|-----------|----------|
| OpenAI | **Responses API** | `/v1/responses` |
| Anthropic | **Messages API** | `/v1/messages` |
| Gemini | **Gemini API** | `/v1beta/...generateContent` |

**Note**: OpenAI's Responses API (not Chat Completions) is the primary target. The Chat Completions API is a compatibility mode that misses features like reasoning tokens.

## Request Translation

Each adapter must translate the unified `Request` into the provider's format:

1. **Messages** → Provider-specific message format
2. **System prompt** → Provider-specific system parameter
3. **Tools** → Provider-specific tool definitions
4. **Tool choice** → Provider-specific tool choice format
5. **Parameters** (temperature, max_tokens, etc.) → Provider-specific parameter names
6. **Provider options** → Pass-through of provider-specific fields

## Per-Provider Differences

| Concern | OpenAI | Anthropic | Gemini |
|---------|--------|-----------|--------|
| System message | `instructions` parameter | `system` parameter | `systemInstruction` |
| Developer role | `developer` role | Merged with system | Merged with system |
| Message alternation | No strict requirement | **Strict user/assistant alternation** | No strict requirement |
| Reasoning tokens | Via `output_tokens_details` | Via thinking blocks (visible text) | Via `thoughtsTokenCount` |
| Tool call IDs | Provider-assigned | Provider-assigned | **No unique IDs** (use function name) |
| Tool result format | Separate `tool` role | `tool_result` blocks in user message | `functionResponse` in user content |
| Tool choice "none" | `"none"` | **Omit tools entirely** | `"NONE"` |
| max_tokens | Optional | **Required** (default to 4096) | Optional (as `maxOutputTokens`) |
| Thinking blocks | Not exposed (o-series internal) | `thinking` / `redacted_thinking` | `thought` parts (2.5 models) |
| Structured output | Native json_schema mode | Prompt engineering or tool extraction | Native responseSchema |
| Streaming protocol | SSE with `data:` lines | SSE with event type + data | SSE (with `?alt=sse`) or JSON |
| Stream termination | `data: [DONE]` | `message_stop` event | Final chunk (no signal) |
| Finish reason for tools | `tool_calls` | `tool_use` | No dedicated reason (infer) |
| Image input | Data URI in `image_url` | `base64` source with `media_type` | `inlineData` with `mimeType` |
| Prompt caching | Automatic (free, 50% discount) | Explicit `cache_control` blocks (90% discount) | Automatic (free prefix caching) |
| Authentication | Bearer token in Authorization | `x-api-key` header | `key` query parameter |
| API versioning | Via URL path (`/v1/`) | `anthropic-version` header | Via URL path (`/v1beta/`) |
| Beta headers | N/A | `anthropic-beta` header | N/A |

## Adding a New Provider

To support a new provider:

1. Implement the `ProviderAdapter` interface with `name`, `complete()`, and `stream()`
2. Write request translation (unified Request → provider format)
3. Write response translation (provider response → unified Response)
4. Write error translation (HTTP errors → error hierarchy)
5. Write streaming translation (provider events → StreamEvent objects)
6. Handle provider quirks (document and handle edge cases)
7. Register the adapter with `Client.from_env()` or programmatic setup

## OpenAI-Compatible Endpoints

Many third-party services (vLLM, Ollama, Together AI, Groq) expose OpenAI-compatible APIs. A separate `OpenAICompatibleAdapter` uses the Chat Completions endpoint (`/v1/chat/completions`) for these:

```python
adapter = OpenAICompatibleAdapter(
    api_key="...",
    base_url="https://my-vllm-instance.example.com/v1"
)
```

This is distinct from the primary OpenAI adapter because third-party services typically only implement Chat Completions, not the Responses API. This adapter does not support reasoning tokens, built-in tools, or other Responses API features.

## See Also

- [four-layer-architecture.md](four-layer-architecture.md) — Provider adapters as Layer 1
- [messages-and-content.md](messages-and-content.md) — The unified message format adapters translate
- [streaming.md](streaming.md) — Stream format translation
- [error-handling-and-retry.md](error-handling-and-retry.md) — Error translation
- [caching-and-middleware.md](caching-and-middleware.md) — Per-provider prompt caching differences
