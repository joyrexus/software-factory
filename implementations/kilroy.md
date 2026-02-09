# Kilroy

*Dan Shapiro's local-first software factory CLI.*

## Overview

Kilroy is a command-line tool that turns English requirements into an [Attractor](../components/attractor/INDEX.md) pipeline (a DOT graph), validates the pipeline, then executes it node-by-node with tool-using coding agents in an isolated git worktree. It is the first known open-source implementation of the Attractor specification.

**Repository:** [github.com/danshapiro/kilroy](https://github.com/danshapiro/kilroy)

## Four-Stage Workflow

### 1. Ingest

Takes English-language requirements — a feature description, a bug report, a refactoring objective — and produces a DOT graph representing the Attractor pipeline. The pipeline defines the sequence of agent tasks, their dependencies, and their validation checkpoints.

### 2. Validate

Checks the generated pipeline for structural correctness: valid DOT syntax, reachable nodes, satisfiable conditions, consistent state references. This stage catches specification errors before any agent work begins, preventing wasted tokens on malformed pipelines.

### 3. Run

Runs the pipeline node-by-node. Each node invokes a coding agent with tool access (file read/write, shell commands, test execution) in an isolated git worktree. The worktree isolation means agent work doesn't affect the main working directory until explicitly merged. Checkpoints and git commits at each node provide recovery points if a later node fails.

### 4. Resume

Recovers interrupted runs from three sources: execution logs, a CXDB context ID, or a run branch in git. Resume enables Kilroy to pick up where it left off after crashes, context-window exhaustion, or manual interruption — a necessary property for long-running pipelines where re-execution from scratch wastes tokens and time.

## CXDB Integration

CXDB serves as Kilroy's execution database, fulfilling three functions:

- **Event recording** — Typed run events (startup, stage completion, checkpointing, final status) are written to CXDB as the pipeline executes, creating a structured execution history.
- **Artifact storage** — Logs, outputs, and archives are stored in CXDB's blob storage via content-addressed deduplication.
- **Recovery** — CXDB metadata (logs_root, checkpoint pointers) enables the Resume stage to reconstruct execution state without re-running completed nodes.

The division of responsibilities is deliberate: git tracks code changes (what was built); CXDB tracks execution history (how it was built). The two systems are complementary — git branches record the artifacts, CXDB records the process that produced them.

## Attractor Spec Comparison

Where the Attractor specification defines abstract contracts, Kilroy makes concrete implementation choices:

| Area | Specification | Kilroy |
|------|---------------|--------|
| Graph DSL | DOT schema, handler model | Go engine parsing and executing DOT graphs |
| Coding-agent loop | Session model, tool behavior contracts | CLI routing to provider-specific agent binaries |
| LLM client | Provider-neutral adapter contracts | Concrete adapters for three providers |
| Checkpointing | Attractor/CXDB contracts | Git branch commits per node, worktree isolation |
| Ingestion | Unspecified | Claude CLI prompt-to-DOT pipeline |
| Recovery | Unspecified | Log replay, CXDB context restore, branch resume |

The comparison illustrates that Kilroy is both an implementation of the spec (graph DSL, coding loop, checkpointing) and an extension of it (ingestion from English, recovery mechanisms). The spec deliberately leaves ingestion and recovery open; Kilroy fills those gaps with opinionated choices.

## Provider Support

Kilroy supports three LLM providers, each invoked through its respective CLI tool:

- **OpenAI** — Codex CLI (`codex exec`)
- **Anthropic** — Claude CLI (`claude -p`)
- **Google** — Gemini CLI (`gemini -p`)

Each provider requires explicit backend configuration and corresponding environment variables. Kilroy supports two execution profiles: a **real** profile (default) that uses canonical binaries and rejects custom paths, and a **test-shim** profile (`--allow-test-shim`) that accepts configured fake executables for testing.

## Notable Design Choices

**Local-first.** Kilroy runs entirely on the developer's machine. No cloud infrastructure, no server dependencies, no API keys beyond the LLM provider. This makes it accessible for individual developers and small teams.

**Git worktree isolation.** Agent work happens in a separate worktree, preserving the developer's main branch. Failed experiments are discarded cleanly. Successful results are merged explicitly.

**Attractor spec compliance.** Kilroy implements the Attractor specification as described in [strongdm/attractor](https://github.com/strongdm/attractor), including DOT-based graph definition, node handlers, and state management.

## Significance

Kilroy demonstrates that the software factory concept is implementable at the individual-developer scale — you don't need a large organization or custom infrastructure to start applying these patterns. It also validates the Attractor specification by providing a working implementation that exercises the spec's design decisions. The CXDB integration shows that structured execution memory and code-change tracking can coexist as complementary systems.

## See Also

- [Attractor](../components/attractor/INDEX.md) — The pipeline orchestrator specification that Kilroy implements
- [Context Store](../components/context-store/INDEX.md) — The context store concept that CXDB implements for Kilroy
- [CXDB](https://github.com/strongdm/cxdb) — StrongDM's open-source context store used as Kilroy's execution database
- [The Seed](../principles/seed.md) — English requirements as seeds for pipeline generation
- [Shift Work](../techniques/shift-work.md) — Kilroy's workflow separates interactive specification from non-interactive execution
