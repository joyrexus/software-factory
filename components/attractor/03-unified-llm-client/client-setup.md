# Client Setup

The `Client` class is the main entry point for making LLM calls. It can be configured from environment variables or programmatically.

## Client.from_env()

The simplest setup — auto-discovers providers from environment variables:

```python
client = Client.from_env()
```

This checks for API keys in environment variables and registers adapters for each provider found:

| Provider | Environment Variable |
|----------|---------------------|
| OpenAI | `OPENAI_API_KEY` |
| Anthropic | `ANTHROPIC_API_KEY` |
| Gemini | `GEMINI_API_KEY` or `GOOGLE_API_KEY` |

If multiple providers are available, the first found becomes the default provider. You can override this:

```python
client = Client.from_env(default_provider="anthropic")
```

## Programmatic Setup

For explicit control:

```python
client = Client(
    providers={
        "anthropic": AnthropicAdapter(api_key="..."),
        "openai": OpenAIAdapter(api_key="..."),
    },
    default_provider="anthropic",
    middleware=[logging_middleware]
)
```

## Model String Convention

Model identifiers use native provider strings — whatever the provider calls the model:

| Provider | Example Model Strings |
|----------|----------------------|
| OpenAI | `gpt-5.2`, `gpt-5.2-mini`, `o3-pro` |
| Anthropic | `claude-opus-4-6`, `claude-sonnet-4-5-20250929` |
| Gemini | `gemini-2.5-pro`, `gemini-2.5-flash` |

The client does **not** maintain its own model naming scheme — it passes model strings through to the provider as-is. This avoids the maintenance burden of tracking provider model names and means new models work immediately.

## Model Catalog

While model strings pass through directly, the client ships a model catalog for discoverability:

```python
# List available models
models = client.list_models()

# Get info about a specific model
info = client.get_model_info("claude-opus-4-6")
# → ModelInfo(id="claude-opus-4-6", provider="anthropic", capabilities=[...])
```

The catalog is **advisory, not restrictive** — unknown model strings still pass through. The catalog exists because AI coding agents building on this SDK often hallucinate model identifiers from stale training data. It gives them a reliable, up-to-date source of valid model IDs.

## Default Client

For convenience, a module-level default client avoids passing the client everywhere:

```python
from llm_client import set_default_client, generate

set_default_client(Client.from_env())

# Now generate() uses the default client automatically
result = generate(model="claude-opus-4-6", prompt="Hello")
```

If no default client is set, the first call to a high-level function triggers lazy initialization via `Client.from_env()`.

## Provider Routing

When making a request, the client determines which adapter to use:

1. If `provider` is specified on the request → use that adapter
2. If not → use the `default_provider`
3. If no default → raise `ConfigurationError`

```python
# Explicit provider
result = client.complete(Request(model="gpt-5.2", provider="openai", ...))

# Default provider
result = client.complete(Request(model="claude-opus-4-6", ...))
```

## See Also

- [four-layer-architecture.md](four-layer-architecture.md) — Where the client sits in the architecture
- [provider-adapters.md](provider-adapters.md) — The adapters the client routes to
- [high-level-api.md](high-level-api.md) — Convenience functions that use the client
