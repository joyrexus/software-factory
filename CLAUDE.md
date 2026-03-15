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
- **[components/](components/README.md)** — Infrastructure deep-dives (Attractor, Context Store, Agent Identity) and known implementations (Kilroy)

Sub-level directories use `README.md` files for navigable overviews, rendered automatically by GitHub when browsing.

## Writing Guidelines

### Concept-First, Not Vendor-First

Write about principles and techniques as general concepts, not as features of a specific product. StrongDM, Factory.ai, and Klaassen are *sources* of ideas, not the subjects. Good: "A specification must identify where the agent begins reading and what artifact proves completion." Bad: "Factory.ai says specifications should..."

### Attribution

When a specific source contributes a key insight, attribute naturally: "As Klaassen observes, agent output quality depends more on the engineering environment than on agent sophistication." Use source names for provenance, not as subjects of sentences. Every attributed source must have a corresponding entry in [SOURCES.md](SOURCES.md).

When citing or referencing a source, link to the relevant entry in SOURCES.md (e.g., `[Willison](../SOURCES.md#simon-willisons-review)`) rather than linking directly to external URLs. SOURCES.md provides summaries and key contributions that contextualize each source within the overall thesis — readers benefit from landing there first. Direct external URLs belong only in SOURCES.md itself.

When adding a new source to SOURCES.md, follow the established entry format: `## Heading` (separated by `---` dividers), `**URL:**` link, optional `**Author:**` line, one or two body paragraphs describing the source's contribution concept-first, `**Key contributions:**` summary list, and `**Tags:**` linking to GLOSSARY.md anchors (e.g., `[term](GLOSSARY.md#term)`).

### Tone

Authoritative but not dogmatic. These are emerging patterns, not settled science. Use language like "the evidence suggests" or "practitioners report" rather than "you must" or "the correct approach is." Acknowledge open questions — Willison's skepticism is a feature, not a problem to overcome.

### Cross-References

Every file should include a "See Also" section linking to related content. Principles link to techniques that implement them. Techniques link to principles they embody and components that use them. Components link back to both.

## Filesystem Technique

- `INDEX.md` — full index, every file reachable in one table (Level 0)
- Directory `README.md` files — navigable overviews (Level 1); rendered by GitHub
- `meta/README.md` — exception: it is the primary conceptual framing, not just navigation
- Individual content files are Level 2

## File Naming

**Structural files** use uppercase names: `INDEX.md`, `README.md`, `SOURCES.md`, `CLAUDE.md`. These are project-level files with special roles (table of contents, entry points, bibliography, project instructions).

**Content files** use lowercase names (e.g., `seed.md`, `paradigm.md`, `maturity-model.md`). This applies to all files that contain subject-matter content.

## Internal Consistency

The rules below apply to the **editable knowledge base**: `meta/`, `principles/`, `techniques/`, and the top-level structural files (`INDEX.md`, `README.md`, `GLOSSARY.md`, `SOURCES.md`).

**`components/` is excluded from these rules.** Its content is largely static reference material. Do not read files inside `components/` when making editorial judgements, checking cross-references, or verifying consistency — the context cost outweighs the benefit. Treat `components/README.md` as a boundary: you may read it for a summary, but do not descend into subdirectories unless the user explicitly asks.

When modifying any file in the editable knowledge base, you must:

1. **Read related documents first.** Before editing a file, read the files it cross-references (check its "See Also" section) and any files that reference it. Understand the surrounding context so your changes don't contradict or duplicate existing content.

2. **Update cross-references.** If you add, rename, move, or remove a file, update every document that links to it. Use `Grep` to find all references to the affected filename. Limit this search to `meta/`, `principles/`, `techniques/`, and top-level files — exclude `components/`.

3. **Update README files.** Every content file must be listed in its directory's `README.md`. If you add a new file, add it to the directory README. If you add a new section, add it to the root `INDEX.md` as well.

4. **Maintain the root INDEX.** The root `INDEX.md` is the authoritative table of contents. Any structural change (new sections, renamed sections, significant scope changes to a section) must be reflected there.

5. **Preserve terminology.** Use terms consistently as defined in [GLOSSARY.md](GLOSSARY.md). If you introduce a new cross-cutting term, add it to GLOSSARY.md.

6. **No orphaned files.** Every markdown file in the editable knowledge base must be reachable from the root INDEX through the directory README chain. After any structural change, verify this — but do not descend into `components/` to check.

7. **Keep READMEs current.** When adding, removing, or renaming files in a subdirectory, update that directory's `README.md` to reflect the change. This maintains navigability and is a practical application of the [Pyramid Summaries](techniques/pyramid-summaries.md) technique — each README is a summary of its directory's contents, regenerable from the files below it.
