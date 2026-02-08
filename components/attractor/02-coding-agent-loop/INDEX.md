# 02 — Coding Agent Loop

The agentic coding loop — a programmable library that pairs LLMs with developer tools to produce working code. This is the engine inside each `codergen` node in an Attractor pipeline.

The loop follows a simple cycle: send a prompt to an LLM, receive tool calls, execute the tools, send results back, and repeat until the task is done or a stop condition is hit.

## Files

1. **[agentic-loop.md](agentic-loop.md)** — Core loop mechanics, session state machine, completion detection
2. **[provider-profiles.md](provider-profiles.md)** — Provider-specific tool sets (OpenAI, Anthropic, Gemini) and shared core tools
3. **[execution-environment.md](execution-environment.md)** — Where tools run: local, Docker, K8s, WASM, SSH
4. **[tools-and-truncation.md](tools-and-truncation.md)** — Output truncation pipeline, per-tool limits, head/tail algorithm
5. **[session-and-config.md](session-and-config.md)** — SessionConfig fields, stop conditions, reasoning effort
6. **[steering-and-context.md](steering-and-context.md)** — Steering queue, follow-up queue, system prompt tiers, project instructions
7. **[loop-detection.md](loop-detection.md)** — Call signature tracking, cycle detection, warning injection
8. **[subagents.md](subagents.md)** — Child session spawning, independent history, depth limiting

## See Also

- [../01-pipeline-orchestrator/node-types-and-handlers.md](../01-pipeline-orchestrator/node-types-and-handlers.md) — How codergen nodes invoke agent sessions
- [../03-unified-llm-client/INDEX.md](../03-unified-llm-client/INDEX.md) — The LLM client the loop uses for model calls
- [../04-implementation-guide/integration-patterns.md](../04-implementation-guide/integration-patterns.md) — Pipeline ↔ Agent Loop integration
