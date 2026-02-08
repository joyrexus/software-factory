# Context Store Architecture

*DAG structure, deduplication, dynamic types, and query patterns.*

## DAG Structure

The context store organizes entries as a directed acyclic graph (DAG). Each entry references zero or more parent entries, creating a history that can branch (when multiple agents work in parallel) and merge (when results are combined). The DAG structure naturally models the workflow: a task entry references the specification entry it was derived from, the validation entries it produced, and the compound knowledge entries it contributed to.

Entries are immutable once written. Updates create new entries that reference the previous version as a parent, preserving full history. This append-only model means no context is ever lost — a critical property for debugging and auditing agent decisions.

## Blob Deduplication

Large artifacts — code files, test results, specification documents — are stored as content-addressed blobs. When multiple entries reference the same artifact, they share a single blob. This is the same pattern used by git: content is stored once, referenced by hash, and deduplicated transparently.

Deduplication matters because a software factory produces enormous volumes of intermediate artifacts. Without it, the context store would grow linearly with every task. With it, storage scales with the number of unique artifacts, not the number of references.

## Dynamic Types

Entries carry type information that describes their schema. Rather than a fixed set of entry types, the context store supports dynamic types — new schemas can be defined as the factory evolves. A "task result" type, a "specification" type, a "validation report" type, and a "compound learning" type might all coexist, each with its own schema and display format.

Dynamic types enable tooling: a visual debugger can render each entry type appropriately, a query engine can filter by type, and validation can check that entries conform to their declared schema.

## Query Patterns

Agents query the context store to find relevant prior work. Common query patterns include:

- **By task lineage** — "What happened the last time we modified this service?"
- **By topic** — "What do we know about OAuth integration?"
- **By recency** — "What compound learnings were captured in the last week?"
- **By dependency** — "What entries depend on this specification?"
- **By type** — "Show me all validation reports for this component."

The query interface must be fast enough that agents can query during the Plan step without significant latency. Context retrieval is part of the inner loop, not a background process.

## Visual Debugging

Humans need to understand what agents are doing and why. A visual debugger renders the DAG as an explorable graph, showing entry relationships, content, types, and timestamps. This is the factory's observability layer for context — analogous to logging for code execution, but structured and queryable.

## See Also

- [Context Store Overview](INDEX.md) — What a context store is and why you need one
- [Attractor: State and Context](../attractor/01-pipeline-orchestrator/state-and-context.md) — How the pipeline orchestrator manages state
- [Compound Knowledge](../../principles/compound-knowledge.md) — The principle driving context store design
