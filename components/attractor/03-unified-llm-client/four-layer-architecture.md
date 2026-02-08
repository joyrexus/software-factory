# Four-Layer Architecture

The Unified LLM Client is organized into four layers, from low-level provider interaction to high-level convenience APIs.

## The Layers

```
┌─────────────────────────────────────────────┐
│  Layer 4: High-Level API                     │
│  generate(), stream(), generate_object()     │
├─────────────────────────────────────────────┤
│  Layer 3: Core Client                        │
│  Client class, routing, middleware chain     │
├─────────────────────────────────────────────┤
│  Layer 2: Utilities                          │
│  Message model, error hierarchy, retry logic │
├─────────────────────────────────────────────┤
│  Layer 1: Provider Spec / Adapters           │
│  OpenAI, Anthropic, Gemini native API calls  │
└─────────────────────────────────────────────┘
```

### Layer 1: Provider Adapters

The lowest layer. Each adapter translates between the unified interface and a provider's native API:
- OpenAI → Responses API (`/v1/responses`)
- Anthropic → Messages API (`/v1/messages`)
- Gemini → Gemini API (`/v1beta/...generateContent`)

See [provider-adapters.md](provider-adapters.md).

### Layer 2: Utilities

Shared data structures and logic used across all layers:
- `Message`, `ContentPart`, `Role` — the unified message model
- `SDKError` hierarchy — typed error classes
- Retry logic with exponential backoff
- Stream event types

### Layer 3: Core Client

The `Client` class — the main entry point:
- Routes requests to the correct provider adapter
- Runs the middleware chain
- Manages default provider configuration
- Handles parallel tool execution

### Layer 4: High-Level API

Convenience functions that wrap the core client:
- `generate()` — simple prompt → response
- `stream()` — streaming response
- `generate_object()` — structured output with schema validation

See [high-level-api.md](high-level-api.md).

## Design Principles

| Principle | Description |
|-----------|-------------|
| **Provider-agnostic** | Callers don't know which provider they're talking to |
| **Streaming-first** | Streaming is a core capability, not an afterthought |
| **Minimal surface** | Small API surface — `complete()` and `stream()` at the core |
| **Composable middleware** | Cross-cutting concerns (logging, caching, rate limiting) via middleware |
| **Escape hatches** | `provider_options` for passing provider-specific parameters |
| **Native APIs** | Uses each provider's best API, not compatibility shims |

## Why Native APIs

The spec explicitly rejects using OpenAI's Chat Completions API as a universal transport:

> The Chat Completions API is an OpenAI-specific protocol that other providers partially mimic as a convenience shim. Using it as the universal transport loses critical capabilities.

Capabilities lost by using compatibility shims:
- OpenAI reasoning tokens (only available via Responses API)
- Anthropic thinking blocks and prompt caching
- Gemini grounding and code execution
- Provider-specific error handling and headers

The unified SDK's value is precisely in abstracting over these **different native APIs**.

## Layer Dependencies

Each layer depends only on the layer(s) below it:
- High-level API → Core Client → Utilities + Provider Adapters
- Provider Adapters → Utilities (for message model, errors)
- Utilities → nothing (pure data structures and algorithms)

This means you can use lower layers directly if the high-level API doesn't fit your use case.

## See Also

- [client-setup.md](client-setup.md) — Setting up the Core Client (Layer 3)
- [provider-adapters.md](provider-adapters.md) — Layer 1 in detail
- [high-level-api.md](high-level-api.md) — Layer 4 in detail
- [caching-and-middleware.md](caching-and-middleware.md) — Middleware pattern (Layer 3)
