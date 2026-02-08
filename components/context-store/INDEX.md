# Context Store

*Persistent structured memory for agents.*

## What It Is

A context store is the persistent, queryable memory layer that agents use to recall previous work, decisions, and learnings. Without it, every agent session starts from zero — re-discovering codebase structure, re-reading specifications, re-learning patterns that previous sessions already figured out. The context store is what makes [compound knowledge](../../principles/compound-knowledge.md) practical at scale.

While the [filesystem-as-memory](../../techniques/filesystem-as-memory.md) technique works for navigable knowledge bases like this one, a software factory producing hundreds of tasks per day needs structured memory with stronger guarantees: deduplication, query support, type safety, and visual debugging.

## Key Properties

A context store for a software factory must be:

- **Append-only** — History is never lost. Every artifact, decision, and learning is preserved in a directed acyclic graph (DAG) of immutable entries.
- **Deduplicated** — Blob-level deduplication ensures that repeated references to the same artifact don't multiply storage costs.
- **Queryable** — Agents can find relevant context by task, by topic, by time range, or by dependency chain, without loading the entire history.
- **Typed** — Entries carry dynamic types that describe their schema, enabling tooling to validate and display context appropriately.
- **Debuggable** — Visual tools let humans inspect the DAG, trace decisions, and understand why an agent made a particular choice.

## Reference Implementation

StrongDM's CXDB (Context Database) is the reference implementation described in the Software Factory materials. It implements the DAG-structured, blob-deduplicated, dynamically-typed model described here.

## Files

| File | Description |
|------|-------------|
| [Architecture](architecture.md) | DAG structure, blob deduplication, dynamic types, query patterns |

## See Also

- [Compound Knowledge](../../principles/compound-knowledge.md) — The principle that motivates a context store
- [Filesystem as Memory](../../techniques/filesystem-as-memory.md) — Lightweight alternative for simpler use cases
- [Attractor](../attractor/INDEX.md) — Pipeline orchestrator that produces context for the store
- [Agent Identity](../agent-identity/INDEX.md) — Access control for who can read and write context
