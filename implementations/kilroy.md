# Kilroy

*Dan Shapiro's local-first software factory CLI.*

## Overview

Kilroy is a command-line tool that turns English requirements into an [Attractor](../components/attractor/INDEX.md) pipeline (a DOT graph), validates the pipeline, then executes it node-by-node with tool-using coding agents in an isolated git worktree. It is the first known open-source implementation of the Attractor specification.

**Repository:** [github.com/danshapiro/kilroy](https://github.com/danshapiro/kilroy)

## Three-Stage Workflow

### 1. Ingest

Takes English-language requirements — a feature description, a bug report, a refactoring objective — and produces a DOT graph representing the Attractor pipeline. The pipeline defines the sequence of agent tasks, their dependencies, and their validation checkpoints.

### 2. Validate

Checks the generated pipeline for structural correctness: valid DOT syntax, reachable nodes, satisfiable conditions, consistent state references. This stage catches specification errors before any agent work begins, preventing wasted tokens on malformed pipelines.

### 3. Execute

Runs the pipeline node-by-node. Each node invokes a coding agent with tool access (file read/write, shell commands, test execution) in an isolated git worktree. The worktree isolation means agent work doesn't affect the main working directory until explicitly merged. Checkpoints and git commits at each node provide recovery points if a later node fails.

## Notable Design Choices

**Local-first.** Kilroy runs entirely on the developer's machine. No cloud infrastructure, no server dependencies, no API keys beyond the LLM provider. This makes it accessible for individual developers and small teams.

**Git worktree isolation.** Agent work happens in a separate worktree, preserving the developer's main branch. Failed experiments are discarded cleanly. Successful results are merged explicitly.

**Attractor spec compliance.** Kilroy implements the Attractor specification as described in [strongdm/attractor](https://github.com/strongdm/attractor), including DOT-based graph definition, node handlers, and state management.

## Significance

Kilroy demonstrates that the software factory concept is implementable at the individual-developer scale — you don't need a large organization or custom infrastructure to start applying these patterns. It also validates the Attractor specification by providing a working implementation that exercises the spec's design decisions.

## See Also

- [Attractor](../components/attractor/INDEX.md) — The pipeline orchestrator specification that Kilroy implements
- [The Seed](../principles/seed.md) — English requirements as seeds for pipeline generation
- [Shift Work](../techniques/shift-work.md) — Kilroy's workflow separates interactive specification from non-interactive execution
