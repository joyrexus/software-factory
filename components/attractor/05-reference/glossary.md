# Glossary

Key terms used across the Attractor specs and Software Factory documentation.

## A

**Accelerator Key** — A keyboard shortcut extracted from edge labels (e.g., `[Y]` in `[Y] Yes`), used for quick human responses in wait.human nodes. See [../01-pipeline-orchestrator/human-in-the-loop.md](../01-pipeline-orchestrator/human-in-the-loop.md).

**Agentic Loop** — The core cycle of the Coding Agent Loop: send prompt → receive tool calls → execute tools → send results → repeat. See [../02-coding-agent-loop/agentic-loop.md](../02-coding-agent-loop/agentic-loop.md).

**Attractor** — The DOT-based pipeline orchestration system. The top layer of the Software Factory stack. Named (presumably) for the mathematical concept of an attractor — a state toward which a system evolves.

## C

**Checkpoint** — A snapshot of pipeline execution state saved after each node completes. Enables crash recovery and resume. See [../01-pipeline-orchestrator/checkpoints-and-resume.md](../01-pipeline-orchestrator/checkpoints-and-resume.md).

**CodergenBackend** — The interface through which the Attractor pipeline invokes coding agent sessions. Abstracts the Coding Agent Loop implementation. See [../04-implementation-guide/integration-patterns.md](../04-implementation-guide/integration-patterns.md).

**Context Fidelity** — Controls how much conversation history carries between pipeline nodes. Modes: `full`, `compact`, `summary:low/medium/high`. See [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md).

**Context Store** — The key-value map shared across pipeline node executions. Handlers read inputs and write outputs via context updates.

**ContentPart** — A tagged union representing one piece of content in a message: text, image, tool call, thinking block, etc. See [../03-unified-llm-client/messages-and-content.md](../03-unified-llm-client/messages-and-content.md).

**CXDB** — Conversation-native observability database. A StrongDM Software Factory product for storing and querying agent conversations.

## D

**DOT DSL** — The Graphviz DOT language subset used to define Attractor pipelines. A plain-text format for directed graphs. See [dot-syntax-quick-ref.md](dot-syntax-quick-ref.md).

**DTU (Digital Twin Universe)** — Behavioral clones of third-party services used for testing at scale. A StrongDM technique for validating agent-produced code against realistic service simulations. See [../07-future-extensions/validation-and-testing.md](../07-future-extensions/validation-and-testing.md).

## E

**Edge** — A transition between nodes in the pipeline graph. Can have conditions, labels, weights, and fidelity attributes.

**ExecutionEnvironment** — The interface abstracting where tools run (local shell, Docker, K8s, etc.). See [../02-coding-agent-loop/execution-environment.md](../02-coding-agent-loop/execution-environment.md).

## F

**Fan-In** — A node (shape=invhouse) that waits for parallel branches to complete and merges their contexts. Counterpart to the Parallel handler.

**Fidelity** — See Context Fidelity.

## G

**Genrefying** — Reorganizing a knowledge structure as understanding deepens. A library-science principle applied to the filesystem technique. The idea that information hierarchies should be restructured to optimize future retrieval as the collection grows.

**Goal Gate** — A node marked with `goal_gate=true` that must reach SUCCESS status before the pipeline can complete. See [../01-pipeline-orchestrator/goal-gates.md](../01-pipeline-orchestrator/goal-gates.md).

## H

**Handler** — An implementation that executes a specific node type. Mapped to nodes by their `shape` attribute. See [../01-pipeline-orchestrator/node-types-and-handlers.md](../01-pipeline-orchestrator/node-types-and-handlers.md).

## I

**Interviewer** — The interface for human-in-the-loop interaction. Implementations: ConsoleInterviewer, AutoApproveInterviewer, QueueInterviewer, etc. See [../01-pipeline-orchestrator/human-in-the-loop.md](../01-pipeline-orchestrator/human-in-the-loop.md).

## M

**Model Stylesheet** — CSS-like rules for assigning LLM models to pipeline nodes. Supports universal, class, and ID selectors. See [../01-pipeline-orchestrator/model-stylesheet.md](../01-pipeline-orchestrator/model-stylesheet.md).

**Middleware** — Functions in the LLM client that wrap requests/responses for cross-cutting concerns (logging, caching, etc.). Uses an onion model. See [../03-unified-llm-client/caching-and-middleware.md](../03-unified-llm-client/caching-and-middleware.md).

## N

**NLSpec (Natural Language Specification)** — A prose document detailed enough to serve as a complete implementation guide for a coding agent. The three NLSpecs define the Attractor stack. See [../00-overview/what-is-attractor.md](../00-overview/what-is-attractor.md).

**Node** — A vertex in the pipeline graph representing a computational task. Each node has a shape (determining its handler), a goal, and optional attributes.

## O

**Outcome** — The result returned by a handler after executing a node. Contains status, context updates, preferred label, artifacts, and error info. See [../01-pipeline-orchestrator/state-and-context.md](../01-pipeline-orchestrator/state-and-context.md).

## P

**Parallel Handler** — A node (shape=hexagon) that fans execution out to multiple concurrent branches. See [../01-pipeline-orchestrator/parallelism.md](../01-pipeline-orchestrator/parallelism.md).

**Provider Adapter** — A component that translates between the unified LLM client interface and a provider's native API. See [../03-unified-llm-client/provider-adapters.md](../03-unified-llm-client/provider-adapters.md).

## S

**Semport** — Semantic import/export system. A StrongDM Software Factory product.

**Session** — A single coding agent run: created with a goal, executes the agentic loop, returns an outcome. See [../02-coding-agent-loop/session-and-config.md](../02-coding-agent-loop/session-and-config.md).

**Shift Work** — StrongDM's term for non-interactive, fully-specified work handed off to agents. Contrasted with interactive pair-programming.

**Software Factory** — StrongDM's vision for AI-driven software production. See [../00-overview/software-factory-context.md](../00-overview/software-factory-context.md).

**Steering** — Messages injected into the agent conversation to redirect behavior mid-session. See [../02-coding-agent-loop/steering-and-context.md](../02-coding-agent-loop/steering-and-context.md).

**StreamAccumulator** — A utility that collects streaming events and builds a complete response. See [../03-unified-llm-client/streaming.md](../03-unified-llm-client/streaming.md).

**Subagent** — A child agent session spawned by a parent session for subtask delegation. Independent history, shared filesystem. See [../02-coding-agent-loop/subagents.md](../02-coding-agent-loop/subagents.md).

## T

**Transform** — An AST modification applied to the parsed graph before validation. See [../01-pipeline-orchestrator/extensibility.md](../01-pipeline-orchestrator/extensibility.md).

**Truncation** — The process of trimming tool output to fit within LLM context limits. Uses head/tail splitting. See [../02-coding-agent-loop/tools-and-truncation.md](../02-coding-agent-loop/tools-and-truncation.md).

## U

**Unified LLM Client** — The provider-agnostic client library at the bottom of the stack. See [../03-unified-llm-client/README.md](../03-unified-llm-client/README.md).
