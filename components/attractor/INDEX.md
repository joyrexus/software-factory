# Index

The Attractor system is StrongDM's Software Factory — a vision for non-interactive, fully-specified coding agents structured as graph-based pipelines. Three NLSpec documents define the stack: a unified LLM client at the bottom, an agentic coding loop in the middle, and a DOT-based pipeline orchestrator on top.

## Table of Contents

### [00-overview/](00-overview/INDEX.md)
What the Software Factory is, what problem Attractor solves, and how the three specs stack together.

### [01-pipeline-orchestrator/](01-pipeline-orchestrator/INDEX.md)
The Attractor pipeline engine: DOT graph definitions, typed node handlers, edge routing, execution lifecycle, state management, checkpointing, retries, goal gates, parallelism, human-in-the-loop, model stylesheets, observability, and extensibility.

### [02-coding-agent-loop/](02-coding-agent-loop/INDEX.md)
The agentic coding loop: LLM ↔ tool cycles, provider-specific tool profiles, execution environments, output truncation, session configuration, steering and context injection, loop detection, and subagent spawning.

### [03-unified-llm-client/](03-unified-llm-client/INDEX.md)
The provider-agnostic LLM client: four-layer architecture, client setup, message and content models, tool calling, streaming, provider adapters (OpenAI / Anthropic / Gemini), error handling, high-level API, caching, and middleware.

### [04-implementation-guide/](04-implementation-guide/INDEX.md)
Practical guidance for building: recommended build sequence, key decision points, integration patterns between the three components, and definition-of-done checklists.

### [05-reference/](05-reference/INDEX.md)
Quick-reference material: glossary of terms, DOT syntax reference card, and condition expression grammar.

### [06-comparisons/](06-comparisons/INDEX.md)
Comparisons with similar systems: a framework for evaluating agent architectures, and a detailed Attractor vs Ramp Inspect analysis.

### [07-future-extensions/](07-future-extensions/INDEX.md)
Forward-looking ideas: advanced validation (DTU), cloud execution environments, collaboration interfaces, and observability/ops integrations.
