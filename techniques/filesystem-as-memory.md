# Filesystem as Memory

*Disk as agent cognition substrate.*

## The Technique

The filesystem is the most natural memory substrate for coding agents. Directories serve as namespaces, filenames as keys, file contents as values, and INDEX files as navigable tables of contents. An agent that can read and write files has persistent, structured, queryable memory — without any additional infrastructure.

This technique is both simple and profound. Agents operate within context windows that are large but finite. The filesystem extends that context indefinitely: the agent reads what it needs, works on it, writes results, and can return later to read again. The filesystem doesn't forget, doesn't hallucinate, and doesn't drift. It's the most reliable memory an agent has.

## This Knowledge Base as Example

This knowledge base itself is an implementation of the filesystem-as-memory technique. The hierarchy:

```
principles/INDEX.md → principles/seed.md
techniques/INDEX.md → techniques/filesystem-as-memory.md
components/INDEX.md → components/attractor/INDEX.md → components/attractor/00-overview/...
```

Each level provides a summary that points to more detailed files. An agent (or human) can navigate from the root INDEX to any specific topic through a chain of increasingly specific overviews. The structure is self-documenting: the hierarchy *is* the knowledge graph.

## Design Principles

**Directories with meaningful names** reflect conceptual hierarchy. A directory called `principles/` tells you what to expect before you read any file in it.

**INDEX files at every level** serve as both navigation and summary. An INDEX file answers the question "what's in this directory and why should I care?" without requiring the reader to open every file.

**On-disk state as source of truth.** The filesystem is not a cache of knowledge stored elsewhere. It *is* the knowledge. Changes to files are changes to the knowledge base. This eliminates synchronization problems between "the real data" and "the documentation."

**Genrefied structure.** The hierarchy can be reorganized as understanding deepens. Files can be moved, directories can be split or merged, and INDEX files can be updated to reflect the new structure. The library-science principle of restructuring information to optimize future retrieval applies directly.

## Implements

- [Agent-Native Environment](../principles/agent-native-environment.md) — Machine-readable, navigable structure designed for agent consumption
- [Compound Knowledge](../principles/compound-knowledge.md) — The filesystem accumulates knowledge across sessions

## See Also

- [Pyramid Summaries](pyramid-summaries.md) — Multi-level compression within the filesystem hierarchy
- [Compound Knowledge](../principles/compound-knowledge.md) — Knowledge accumulation as explicit practice
- [Context Store](../components/context-store/INDEX.md) — When the filesystem isn't enough: structured persistent memory
