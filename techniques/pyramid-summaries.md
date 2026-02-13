# Pyramid Summaries

*Reversible multi-level context compression.*

## The Technique

Pyramid summaries organize information at multiple zoom levels — a one-line description, a paragraph overview, a detailed treatment — so that agents (and humans) can navigate from broad to specific without losing the ability to zoom back out. Each level of the pyramid is a complete, coherent summary at its resolution, not a truncation of the level below.

This technique addresses a fundamental constraint of agentic work: context windows are large but finite, and agents must constantly decide how much detail to load. A pyramid structure lets the agent start at the top (the root INDEX, directory READMEs) and drill into specifics only when needed, keeping the context window focused on relevant detail.

## The Pyramid Structure

```
Level 0: Root INDEX     — One line per section
Level 1: Directory README — One paragraph per file
Level 2: File           — Full treatment of a single topic
Level 3: Appendix       — Extended detail, examples, edge cases
```

Each level is **reversible** — you can reconstruct the higher-level summary from the lower-level content. This means the pyramid can be regenerated as content changes, keeping summaries in sync with details.

## This Knowledge Base as Example

This knowledge base uses pyramid summaries throughout:

- **Root INDEX.md** lists four sections with one-line descriptions
- **Directory README files** (e.g., `principles/README.md`) describe each file in the section with a sentence or two
- **Individual files** (e.g., this one) provide the full treatment
- **Cross-references** connect related topics across the hierarchy

An agent navigating this knowledge base reads the root INDEX to orient, reads a directory README to find the relevant file, and reads the file to get the detail. Total context consumed: three files instead of thirty.

## Implements

- [Compound Knowledge](../principles/compound-knowledge.md) — Summaries make accumulated knowledge navigable
- [Agent-Native Environment](../principles/agent-native-environment.md) — Multi-level structure designed for agent context management

## See Also

- [Filesystem as Memory](filesystem-as-memory.md) — The storage substrate that pyramid summaries organize
- [Progressive Disclosure](progressive-disclosure.md) — The general technique that pyramid summaries implement
- [Context Store](../components/context-store/README.md) — Structured memory when pyramid summaries in files aren't sufficient
