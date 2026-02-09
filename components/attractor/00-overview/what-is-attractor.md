# What Is Attractor

Attractor is a declarative pipeline orchestrator that represents multi-stage AI coding workflows as directed graphs using Graphviz DOT syntax.

## The Problem

AI-powered software workflows require multiple LLM calls chained together with conditional logic, human approvals, and parallel execution. A single prompt-response cycle isn't enough — you need a structured pipeline: generate code, validate it, fix failures, get approval, deploy. Each stage has different requirements (different models, different tools, different failure handling), and the transitions between stages depend on outcomes.

## The Core Idea

Attractor models these workflows as **directed graphs**:

- **Nodes** are computational tasks (LLM calls, human approvals, conditional routing, parallel fan-out)
- **Edges** are transitions with conditions, weights, and labels
- **The DOT file** is the pipeline definition — human-readable, version-controllable, and immediately visualizable with standard Graphviz tools

This is a **non-interactive** agent. Unlike conversational coding assistants (Claude Code, Cursor, etc.), Attractor handles fully-specified work that doesn't need real-time human guidance. Think of it as "shift work" — you define the job, hand it off, and it runs to completion (or surfaces specific decision points via the human-in-the-loop pattern).

## The Three NLSpec Components

Attractor is defined across three Natural Language Specifications (NLSpecs) — prose documents detailed enough for a coding agent to implement directly:

| Spec | Role | Analogy |
|------|------|---------|
| **[Unified LLM Client](../03-unified-llm-client/README.md)** | Provider-agnostic access to OpenAI, Anthropic, and Gemini | The engine |
| **[Coding Agent Loop](../02-coding-agent-loop/README.md)** | Agentic loop pairing LLMs with developer tools | The driver |
| **[Attractor Pipeline](../01-pipeline-orchestrator/README.md)** | DOT-based orchestration of multi-phase workflows | The road map |

Each spec is self-contained and independently implementable, but they're designed to compose: the pipeline orchestrator creates coding agent sessions, which use the LLM client to talk to models.

## What NLSpec Means

An NLSpec (Natural Language Specification) is a prose document detailed enough to serve as a complete implementation guide for a coding agent. Instead of writing code directly, you write a specification that describes data structures, algorithms, interfaces, and validation criteria in natural language. The coding agent reads this spec and produces a working implementation.

This is the Software Factory's core technique: **specify, don't code**. The specs in the `strongdm/attractor` repository are the canonical example.

## See Also

- [software-factory-context.md](software-factory-context.md) — The broader vision this fits into
- [architecture-overview.md](architecture-overview.md) — How the three components stack
