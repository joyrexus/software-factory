# Progressive Disclosure

*Give agents a map, not a manual.*

## The Technique

Agents operate in finite context windows. Flooding them with everything produces worse results than giving them a small, stable entry point and teaching them where to look. Progressive disclosure means layered context — each layer self-contained, each pointing to deeper sources — so that agents load only what a given task requires.

The instinct to help agents by giving them more context is understandable and wrong. A monolithic context document creates four failure modes:

1. **Context scarcity** — a giant file crowds out the task itself and the relevant code. The context window is zero-sum: every token spent on instructions is a token not available for reasoning about the actual work.
2. **Non-guidance** — when everything is "important," agents pattern-match locally instead of navigating intentionally. A wall of rules produces compliance with whichever rule the model happens to attend to, not systematic adherence to all of them.
3. **Instant rot** — a monolithic manual becomes a graveyard of stale rules. No single person maintains the entire document, sections contradict each other, and agents follow outdated instructions with high confidence.
4. **Unverifiability** — a single blob does not support mechanical checks for coverage, freshness, or cross-linking. You cannot lint a monolith for internal consistency the way you can lint a structured directory.

Progressive disclosure solves these by replacing the monolith with a navigable hierarchy: small entry points that teach the agent where to look, structured directories that contain the actual knowledge, and mechanical checks that keep the whole system current.

## The Map Architecture

The concrete implementation is an AGENTS.md file of roughly 100 lines — not a manual, but a table of contents. It answers three questions: how to build, test, and lint; how the codebase is organized; and what evidence must accompany a pull request. For everything beyond those basics, it points into a structured `docs/` directory:

```
docs/
├── design-docs/       — Architectural decisions and rationale
├── exec-plans/        — Active and completed execution plans
├── product-specs/     — Feature specifications and requirements
├── references/        — API contracts, schema definitions, conventions
└── quality-scores/    — Per-module quality grades and trend data
```

Each directory contains focused documents at a consistent level of detail. An agent working on a feature reads AGENTS.md (small, stable), navigates to the relevant product spec (moderate detail), and may drill into a design doc or reference (full detail) — loading context incrementally rather than all at once.

Plans are first-class versioned artifacts within this structure. Execution plans with progress and decision logs are checked into the repository alongside active work, completed work, and known technical debt. An agent picking up a task can read the plan, understand what has been done, what remains, and what decisions were made along the way — without relying on external project management tools or human memory ([Harness Engineering](../SOURCES.md#harness-engineering)).

This knowledge base itself applies the same pattern: a root [INDEX.md](../INDEX.md) serves as the map, directory README files provide intermediate summaries, and individual files contain full treatments. The structure is navigable at every level without requiring the reader to load the entire corpus.

## Mechanical Freshness

A structured knowledge base is only valuable if it stays current. Progressive disclosure supports mechanical freshness in ways a monolith cannot:

**Linters and CI jobs** validate that the knowledge base is internally consistent — cross-links resolve, every file is reachable from the root, required sections exist, and formatting is standardized. These checks run on every commit, catching rot before it accumulates.

**Documentation gardening agents** — recurring background tasks that scan for stale documentation and open fix-up PRs. A gardening agent can check whether a design doc still matches the code it describes, whether a quality score reflects current test coverage, or whether a reference document covers all current API endpoints. Most of these PRs are small, reviewable in under a minute, and eligible for automerge.

**The promotion pattern** closes the loop: when documentation proves insufficient — an agent misinterprets an instruction, a review catches a recurring mistake — the fix is not just a documentation update but a promotion into code. A vague guideline becomes a lint rule. A naming convention becomes a structural test. A "please don't" becomes a mechanical constraint. As [Harness Engineering](../SOURCES.md#harness-engineering) describes it: "when documentation falls short, promote the rule into code."

## Implements

- [Agent-Native Environment](../principles/agent-native-environment.md) — context management as environment design
- [Compound Knowledge](../principles/compound-knowledge.md) — the knowledge base improves through mechanical maintenance

## See Also

- [Pyramid Summaries](pyramid-summaries.md) — a specific implementation: reversible multi-level compression
- [Filesystem as Memory](filesystem-as-memory.md) — the underlying substrate
- [Specification Discipline](specification-discipline.md) — progressive disclosure applied to task specifications
