# 03 — Unified LLM Client

A provider-agnostic client library for accessing OpenAI, Anthropic, and Gemini through a single interface. This is the foundation layer — everything else in the stack talks to models through this client.

The spec emphasizes using each provider's **native API** (OpenAI Responses API, Anthropic Messages API, Gemini API) rather than compatibility shims, ensuring access to provider-specific capabilities like reasoning tokens, thinking blocks, and prompt caching.

## Files

1. **[four-layer-architecture.md](four-layer-architecture.md)** — Provider spec → Utilities → Core client → High-level API
2. **[client-setup.md](client-setup.md)** — Client.from_env(), programmatic setup, model string conventions
3. **[messages-and-content.md](messages-and-content.md)** — Message record, Role enum, ContentPart tagged union
4. **[tool-calling.md](tool-calling.md)** — Tool definition, active vs passive tools, parallel execution, tool choice modes
5. **[streaming.md](streaming.md)** — Stream event types, start/delta/end pattern, StreamAccumulator
6. **[provider-adapters.md](provider-adapters.md)** — ProviderAdapter interface, per-provider API differences, translation steps
7. **[error-handling-and-retry.md](error-handling-and-retry.md)** — Error taxonomy, SDKError hierarchy, exponential backoff, Retry-After handling
8. **[high-level-api.md](high-level-api.md)** — generate(), stream(), generate_object() convenience functions
9. **[caching-and-middleware.md](caching-and-middleware.md)** — Prompt caching per provider, middleware onion model

## See Also

- [../00-overview/architecture-overview.md](../00-overview/architecture-overview.md) — Where the LLM client sits in the stack
- [../02-coding-agent-loop/INDEX.md](../02-coding-agent-loop/INDEX.md) — The agent loop that uses this client
- [../04-implementation-guide/build-sequence.md](../04-implementation-guide/build-sequence.md) — Why this is built first
