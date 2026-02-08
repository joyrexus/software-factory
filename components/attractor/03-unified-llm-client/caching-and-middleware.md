# Caching and Middleware

The client supports prompt caching (per-provider) and a composable middleware pattern for cross-cutting concerns.

## Prompt Caching

Each provider handles prompt caching differently:

| Provider | Mechanism | Configuration | Discount |
|----------|-----------|---------------|----------|
| **OpenAI** | Automatic via Responses API | None needed | 50% on cached tokens (free) |
| **Anthropic** | Explicit `cache_control` breakpoints | Adapter auto-injects | 90% on cached tokens |
| **Gemini** | Automatic prefix caching | None needed | Free |

### OpenAI Caching

Caching happens automatically when using the Responses API. No client-side configuration is required. The savings appear in `Usage.cache_read_tokens`.

### Anthropic Caching

Anthropic requires explicit `cache_control` markers in the request. The adapter **automatically injects** these at optimal points:

1. End of the system prompt
2. End of tool definitions
3. End of the conversation prefix (for multi-turn sessions)

The `prompt-caching-2024-07-31` beta header is included automatically when cache_control is present.

To disable automatic caching:

```python
request = Request(
    ...,
    provider_options={"anthropic": {"auto_cache": False}}
)
```

### Gemini Caching

Like OpenAI, prefix caching works automatically. Savings appear in `Usage.cache_read_tokens` (populated from `usageMetadata.cachedContentTokenCount`).

### Caching Impact

In multi-turn agentic sessions (5+ turns), prompt caching is critical:

> Verify that turn 5+ shows significant cache_read_tokens (>50% of input tokens) for all three providers.

Without caching, each turn re-sends the entire conversation history at full price. With caching, only the new content costs full price.

## Middleware Pattern

The client supports an onion-model middleware chain:

```
Request → [Middleware 1 → [Middleware 2 → [Provider Adapter] ← ] ← ] ← Response
```

### Middleware Signature

```python
def middleware(request: Request, next: Callable) -> Response:
    # Pre-processing: inspect/modify request
    modified_request = transform(request)

    # Call next middleware (or adapter)
    response = next(modified_request)

    # Post-processing: inspect/modify response
    return transform(response)
```

### Execution Order

- **Request phase**: Middleware executes in registration order (first registered → first to see request)
- **Response phase**: Middleware executes in reverse order (last registered → first to see response)

This is the standard onion/layered middleware model.

### Common Middleware Use Cases

| Middleware | Purpose |
|-----------|---------|
| **Logging** | Log request/response metadata, latency, token usage |
| **Rate limiting** | Client-side rate limiting to stay under provider quotas |
| **Cost tracking** | Track token usage and estimated cost per request |
| **Caching** | Cache identical requests to avoid redundant API calls |
| **Request modification** | Add default parameters, inject headers |
| **Metrics** | Emit metrics for monitoring (latency histograms, error rates) |
| **Fallback** | Try a different provider if the primary fails |

### Example: Logging Middleware

```python
def logging_middleware(request, next):
    start_time = time.time()
    log.info(f"LLM request: provider={request.provider} model={request.model}")

    response = next(request)

    elapsed = time.time() - start_time
    log.info(f"LLM response: tokens={response.usage.total_tokens} latency={elapsed:.2f}s")

    return response

client = Client(
    providers={"anthropic": AnthropicAdapter(...)},
    middleware=[logging_middleware]
)
```

### Example: Fallback Middleware

```python
def fallback_middleware(request, next):
    try:
        return next(request)
    except ProviderError:
        # Fall back to a different provider
        fallback_request = request.copy(provider="openai", model="gpt-5.2")
        return next(fallback_request)
```

## See Also

- [four-layer-architecture.md](four-layer-architecture.md) — Middleware in the architecture
- [client-setup.md](client-setup.md) — Configuring middleware on the client
- [provider-adapters.md](provider-adapters.md) — Per-provider caching differences
- [error-handling-and-retry.md](error-handling-and-retry.md) — Retries as a form of middleware
