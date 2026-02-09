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

## Related Approaches

Three external perspectives illuminate different facets of context store design.

### OpenClaw's Memory System

OpenClaw uses plain markdown files as its source of truth — daily session logs plus a long-term `MEMORY.md` — with hybrid retrieval combining vector similarity and BM25 full-text search. The contrasts with the context store model are instructive: OpenClaw is file-first and human-readable; the context store is structured and machine-queryable. OpenClaw uses semantic search to surface relevant history; the context store uses typed queries over a DAG. Both share the insight that disk — not in-memory state — is the authoritative record. OpenClaw's model suits interactive, single-agent sessions where a human reviews and edits memory files directly. The context store targets multi-agent factories at higher throughput, where structured deduplication and typed queries are necessary to manage volume.

**Source:** [OpenClaw Memory Concepts](https://docs.openclaw.ai/concepts/memory)

### Agentic Search over RAG

The argument, articulated by Nicolas Bustamante, is that RAG's chunking and embedding pipeline introduces compounding errors — lossy chunking fragments context, embedding collapse loses relational structure, and retrieval returns fragments without the connections that made them meaningful. Agents with tool access (grep, glob, file reads) navigate relationships more effectively because they follow references rather than matching vectors.

The implications for context store design: the typed, queryable DAG avoids chunking fragmentation by preserving entries as coherent units with explicit parent references. Agents query by lineage, topic, type, or dependency rather than by vector similarity. The context store complements agentic search rather than replacing it — agents use filesystem tools for codebase navigation and the context store for workflow memory across sessions.

**Source:** [The RAG Obituary](https://www.nicolasbustamante.com/p/the-rag-obituary-killed-by-agents)

### S3-First Architecture

The pattern, also from Bustamante, of treating S3 as the authoritative store with databases serving as query indexes over it, and of filesystem/bash as agent-native primitives. The parallel to CXDB's blob CAS is direct: content-addressed immutable storage with metadata indexed separately for queries. The [filesystem-as-memory](../../techniques/filesystem-as-memory.md) technique is the lightweight version of this same insight — the filesystem is the source of truth, with structured tools built on top.

Both perspectives reinforce that agents work well with durable, content-addressed storage over transient database state. The context store's architecture reflects this: immutable blobs for durability, a DAG index for queryability, and content addressing for deduplication.

**Source:** [Lessons from Building AI Agents](https://www.nicolasbustamante.com/p/lessons-from-building-ai-agents-for)

## CXDB

StrongDM's open-source implementation of the context store concept.

**Repository:** [github.com/strongdm/cxdb](https://github.com/strongdm/cxdb)

**What it is.** An AI context store — fast, branch-friendly storage for conversation histories and tool outputs with content-addressed deduplication. CXDB is the reference implementation described in StrongDM's Software Factory materials.

**Turn DAG.** Immutable turn nodes with parent references form a DAG. Each turn carries a `turn_id`, `parent_turn_id`, `depth`, and `payload_hash`. A Context is a mutable pointer to a turn, enabling O(1) forking without data duplication. This mirrors the DAG structure described above, with turns as the concrete entry type.

**Blob CAS.** Payloads are stored as content-addressed blobs using BLAKE3 hashing and Zstd compression. Identical content is stored once regardless of how many turns reference it — the deduplication principle made concrete.

**Type system.** Numeric field tags with registry bundles that publish type descriptors. The system supports forward-compatible schema evolution: new fields can be added without breaking existing readers. Typed payloads can be projected from msgpack to JSON on read.

**Architecture.** Three-tier: React frontend on `:3000`, Go gateway on `:8080`, and a Rust storage server with a binary protocol on `:9009` for writers and HTTP/JSON on `:9010` for readers. The separation of write and read protocols reflects the access pattern — append-heavy ingestion requires low-overhead binary writes; human inspection and agent queries use JSON.

**API surface.** The binary protocol is optimized for append-heavy writes from agents and pipelines. The HTTP/JSON interface supports reads with pagination, filtering, and both typed and raw views.

## See Also

- [Context Store Overview](INDEX.md) — What a context store is and why you need one
- [Attractor: State and Context](../attractor/01-pipeline-orchestrator/state-and-context.md) — How the pipeline orchestrator manages state
- [Compound Knowledge](../../principles/compound-knowledge.md) — The principle driving context store design
- [OpenClaw Memory Concepts](https://docs.openclaw.ai/concepts/memory) — File-first agent memory with hybrid retrieval
- [The RAG Obituary](https://www.nicolasbustamante.com/p/the-rag-obituary-killed-by-agents) — The case for agentic search over embedding-based retrieval
- [Lessons from Building AI Agents](https://www.nicolasbustamante.com/p/lessons-from-building-ai-agents-for) — S3-first architecture and filesystem as agent primitive
- [CXDB](https://github.com/strongdm/cxdb) — StrongDM's open-source context store implementation
