# Project Instructions

This is a hierarchical markdown knowledge base about building a software factory — an engineering organization where coding agents produce and validate software under human architectural direction. It synthesizes ideas from four sources into an integrated, navigable reference.

There is no code — only interconnected markdown files organized as a navigable hierarchy.

## Project Layout

See [INDEX.md](INDEX.md) for the full project layout and master table of contents. Key top-level structure:

- **[INDEX.md](INDEX.md)** — Master table of contents (every file reachable from here)
- **[README.md](README.md)** — The Software Factory Thesis
- **[GLOSSARY.md](GLOSSARY.md)** — Cross-cutting vocabulary (30 terms with anchor IDs for linking)
- **[SOURCES.md](SOURCES.md)** — Annotated bibliography (each source tagged with glossary terms)
- **[CLAUDE.md](CLAUDE.md)** — Project instructions (this file)
- **[meta/](meta/README.md)** — Core context for the thesis: paradigm, open questions, and community commentary (README serves as the primary contextual framing, not just navigation)
- **[principles/](principles/README.md)** — Seven principles behind the concept
- **[techniques/](techniques/README.md)** — Nine repeatable patterns
- **[components/](components/README.md)** — Infrastructure deep-dives (Attractor, Context Store, Agent Identity)
- **[implementations/](implementations/README.md)** — Known implementations

Sub-level directories use `README.md` files for navigable overviews, rendered automatically by GitHub when browsing.

## Writing Guidelines

### Concept-First, Not Vendor-First

Write about principles and techniques as general concepts, not as features of a specific product. StrongDM, Factory.ai, and Klaassen are *sources* of ideas, not the subjects. Good: "A specification must identify where the agent begins reading and what artifact proves completion." Bad: "Factory.ai says specifications should..."

### Attribution

When a specific source contributes a key insight, attribute naturally: "As Klaassen observes, agent output quality depends more on the engineering environment than on agent sophistication." Use source names for provenance, not as subjects of sentences. Every attributed source must have a corresponding entry in [SOURCES.md](SOURCES.md).

When citing or referencing a source, link to the relevant entry in SOURCES.md (e.g., `[Willison](../SOURCES.md#simon-willisons-review)`) rather than linking directly to external URLs. SOURCES.md provides summaries and key contributions that contextualize each source within the overall thesis — readers benefit from landing there first. Direct external URLs belong only in SOURCES.md itself.

### Tone

Authoritative but not dogmatic. These are emerging patterns, not settled science. Use language like "the evidence suggests" or "practitioners report" rather than "you must" or "the correct approach is." Acknowledge open questions — Willison's skepticism is a feature, not a problem to overcome.

### Cross-References

Every file should include a "See Also" section linking to related content. Principles link to techniques that implement them. Techniques link to principles they embody and components that use them. Components link back to both.

## Filesystem Technique

This hierarchy follows StrongDM's own [filesystem technique](https://factory.strongdm.ai/techniques/filesystem):

- **Root `INDEX.md`** is the full quick-reference index for the entire knowledge base — every file reachable in one table
- **Sub-level `README.md` files** provide navigable overviews at each directory level, rendered by GitHub when browsing
- **`meta/README.md`** is an exception to the typical navigational role — it serves as the core contextual piece for understanding the thesis and the conversation being explored, providing the essential framing that other content files elaborate on
- **On-disk state** serves as a practical memory substrate — the filesystem itself is the knowledge graph
- This is a lightweight application of the [Pyramid Summaries](techniques/pyramid-summaries.md) technique: root INDEX is Level 0, directory READMEs are Level 1, individual files are Level 2

The structure is designed to be **genrefied** — reorganized as understanding deepens, following the library-science principle of restructuring information to optimize future retrieval.

## File Naming

**Structural files** use uppercase names: `INDEX.md`, `README.md`, `SOURCES.md`, `CLAUDE.md`. These are project-level files with special roles (table of contents, entry points, bibliography, project instructions).

**Content files** use lowercase names (e.g., `seed.md`, `paradigm.md`, `maturity-model.md`). This applies to all files that contain subject-matter content.

## Living Document

This is not a finished artifact. It's a starting point intended to be iterated upon:

- Sections can be expanded, restructured, or split as specific areas require deeper analysis
- Implementation decisions, spikes, and learnings should be captured back into this hierarchy
- The components section is expected to grow as more factory infrastructure is documented

## Internal Consistency

When modifying any file in this project, you must:

1. **Read related documents first.** Before editing a file, read the files it cross-references (check its "See Also" section) and any files that reference it. Understand the surrounding context so your changes don't contradict or duplicate existing content.

2. **Update cross-references.** If you add, rename, move, or remove a file, update every document that links to it. Use `Grep` to find all references to the affected filename.

3. **Update README files.** Every content file must be listed in its directory's `README.md`. If you add a new file, add it to the directory README. If you add a new section, add it to the root `INDEX.md` as well.

4. **Maintain the root INDEX.** The root `INDEX.md` is the authoritative table of contents. Any structural change (new sections, renamed sections, significant scope changes to a section) must be reflected there.

5. **Preserve terminology.** Use terms consistently as defined in [GLOSSARY.md](GLOSSARY.md) (cross-cutting terms) and [components/attractor/05-reference/glossary.md](components/attractor/05-reference/glossary.md) (Attractor-specific terms). If you introduce a new cross-cutting term, add it to GLOSSARY.md; if it's Attractor-specific, add it to the Attractor glossary.

6. **No orphaned files.** Every markdown file must be reachable from the root INDEX through the directory README chain. After any structural change, verify this.

7. **Keep READMEs current.** When adding, removing, or renaming files in a subdirectory, update that directory's `README.md` to reflect the change. This maintains navigability and is a practical application of the [Pyramid Summaries](techniques/pyramid-summaries.md) technique — each README is a summary of its directory's contents, regenerable from the files below it.
