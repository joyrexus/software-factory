# Attractor — Pipeline Orchestrator

A comprehensive knowledge base produced from a systematic review of StrongDM's [Attractor](https://github.com/strongdm/attractor) specification — the DOT-based pipeline orchestrator at the heart of the [Software Factory](https://factory.strongdm.ai/) concept.

This is one component within the broader [software factory knowledge base](../../README.md). For the full picture — principles, techniques, and other components — start at the [root](../../README.md).

## How This Discovery Was Produced

This knowledge base was generated through a deep review of:

- **[attractor-spec.md](https://github.com/strongdm/attractor/blob/main/attractor-spec.md)** — The DOT-based pipeline orchestrator
- **[coding-agent-loop-spec.md](https://github.com/strongdm/attractor/blob/main/coding-agent-loop-spec.md)** — The programmable coding agent library
- **[unified-llm-spec.md](https://github.com/strongdm/attractor/blob/main/unified-llm-spec.md)** — The provider-agnostic LLM client
- **[factory.strongdm.ai](https://factory.strongdm.ai/)** — Principles, techniques, and products describing the Software Factory vision

Content was extracted, decomposed by topic, and organized into a navigable hierarchy using Claude Code as the research and authoring agent.

## Contributing

See **[../../CLAUDE.md](../../CLAUDE.md)** for project structure conventions, the filesystem technique, and consistency rules for making changes.

---

## Table of Contents

The Attractor system is StrongDM's Software Factory — a vision for non-interactive, fully-specified coding agents structured as graph-based pipelines. Three NLSpec documents define the stack: a unified LLM client at the bottom, an agentic coding loop in the middle, and a DOT-based pipeline orchestrator on top.

### [00-overview/](00-overview/README.md)
What the Software Factory is, what problem Attractor solves, and how the three specs stack together.

### [01-pipeline-orchestrator/](01-pipeline-orchestrator/README.md)
The Attractor pipeline engine: DOT graph definitions, typed node handlers, edge routing, execution lifecycle, state management, checkpointing, retries, goal gates, parallelism, human-in-the-loop, model stylesheets, observability, and extensibility.

### [02-coding-agent-loop/](02-coding-agent-loop/README.md)
The agentic coding loop: LLM ↔ tool cycles, provider-specific tool profiles, execution environments, output truncation, session configuration, steering and context injection, loop detection, and subagent spawning.

### [03-unified-llm-client/](03-unified-llm-client/README.md)
The provider-agnostic LLM client: four-layer architecture, client setup, message and content models, tool calling, streaming, provider adapters (OpenAI / Anthropic / Gemini), error handling, high-level API, caching, and middleware.

### [04-implementation-guide/](04-implementation-guide/README.md)
Practical guidance for building: recommended build sequence, key decision points, integration patterns between the three components, and definition-of-done checklists.

### [05-reference/](05-reference/README.md)
Quick-reference material: glossary of terms, DOT syntax reference card, and condition expression grammar.

### [06-comparisons/](06-comparisons/README.md)
Comparisons with similar systems: a framework for evaluating agent architectures, and a detailed Attractor vs Ramp Inspect analysis.

### [07-future-extensions/](07-future-extensions/README.md)
Forward-looking ideas: advanced validation (DTU), cloud execution environments, collaboration interfaces, and observability/ops integrations.
